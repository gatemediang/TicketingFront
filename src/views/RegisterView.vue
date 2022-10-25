<template>
  <CenterCard>
    <Form class="w-full" @submit.prevent="register">
      <FormGroup
        type="text"
        label="Full Name"
        v-model="form.name"
        :error="errorBag.name"
      />
      <FormGroup
        type="email"
        label="Email"
        v-model="form.email"
        :error="errorBag.email"
      />
      <FormGroup
        type="password"
        label="Password"
        v-model="form.password"
        :error="errorBag.password"
      />
      <FormGroup
        type="password"
        label="Confirm Password"
        v-model="form.confirm_password"
        :error="errorBag.confirm_password"
      />
      <div class="text-center">
        <Button type="submit" class="w-36">Register</Button>
      </div>
    </Form>
  </CenterCard>
</template>

<script setup>
import CenterCard from "../components/organisms/CenterCard.vue"
import { FormGroup, Form, Button } from "@adiranids/vue3-tailwind"
import { reactive } from "vue"
import axios from "axios"
import { api } from "../assets/api"

const errorBag = reactive({
  email: "",
  password: "",
  confirm_password: "",
  name: "",
})

const form = reactive({
  email: "",
  password: "",
  confirm_password: "",
  name: "",
})

function register() {
  if (form.email == "") errorBag.email = "Please enter your email"
  else errorBag.email = ""

  if (form.password == "") errorBag.password = "Please enter your password"
  else errorBag.password = ""

  if (form.name == "") errorBag.name = "Please enter your full name"
  else errorBag.name = ""

  if (form.confirm_password != form.password)
    errorBag.confirm_password =
      "You enter diferent values for password and confirm password field"
  else errorBag.confirm_password = ""

  if (
    form.confirm_password != "" &&
    form.password != "" &&
    form.email != "" &&
    form.name != ""
  ) {
    errorBag.email = ""
    errorBag.password = ""
    form.confirm_password = ""
    errorBag.name = ""
  }

  api()
    .post("/register", form)
    .then((response) => {
      if (response.status == 200 && response.data.token) {
        window.localStorage.setItem("user", response.data.id)
        console.log("response", response.data)
      } else {
        alert("something went wrong")
      }
    })
    .catch((err) => {
      if (err.response.data) {
        alert(err.response.data.detail)
      }
    })

  // axios.post("http://localhost:8000", form).then((response) => {

  // })
}
</script>

<style scoped></style>
