<template>
  <CenterCard>
    <Form class="w-full" @submit.prevent="register">
      <FormGroup type="text" label="Full Name" v-model="form.name" :error="errorBag.name" />
      <FormGroup type="email" label="Email" v-model="form.email" :error="errorBag.email" />
      <FormGroup type="password" label="Password" v-model="form.password" :error="errorBag.password" />
      <FormGroup type="password" label="Confirm Password" v-model="form.confirm_password" :error="errorBag.confirm_password" />
      <FormGroup type="text" label="Role" v-model="form.role" :error="errorBag.role" />
      <div class="text-center">
        <Button type="submit" class="w-36">Register</Button>
      </div>
    </Form>
  </CenterCard>
</template>

<script setup>
import CenterCard from "../components/organisms/CenterCard.vue";
import { FormGroup, Form, Button } from "@adiranids/vue3-tailwind";
import { reactive } from "@vue/reactivity";
import axios from "axios"
import { api } from "../assets/api"
import { useRouter } from "vue-router"

const errorBag = reactive({
  email: "",
  password: "",
  confirm_password:"",
  name: "",
  role: ""
});

const form = reactive({
  email: "",
  password: "",
  confirm_password:"",
  name: "",
  role: ""
});

const router = useRouter()

function register() {
  if (form.email == "") 
  errorBag.email = "Please enter your email"
  else 
  errorBag.email = ""

  if (form.password == "") 
  errorBag.password = "Please enter your password"
  else 
  errorBag.password = ""

  if (form.confirm_password == "") 
  errorBag.confirm_password = "Please confirm your password"
  else 
  errorBag.confirm_password = ""

  if (form.name == "") 
  errorBag.name = "Please enter your full name"
  else 
  errorBag.name = ""

  if (form.role == "") 
  errorBag.role = "Please enter your role"
  else 
  errorBag.role = ""

  if (form.password != "" && form.confirm_password != "" && form.email != "" && form.name != "" && form.role != "") {
    errorBag.email = ""
    errorBag.password = ""
    errorBag.confirm_password = ""
    errorBag.name = ""
    errorBag.role = ""
  }

  api().post("/register", form).then((response) => {
    if (response.status == 200) {
      window.localStorage.setItem("user", response.data.id)
      router.replace("/dashboard")
      }
      else 
      {
        alert("something went wrong")
      }

    }).catch(err =>{
      if(err.response.data)
      {
        alert(err.response.data.detail[0].msg)
      }

    })

  console.log("I am after request")

}
</script>

<style scoped></style>