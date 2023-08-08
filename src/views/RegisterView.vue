
<script setup>
import { reactive, ref } from 'vue';
import { useRouter, RouterLink } from 'vue-router';

  const userCredentials = reactive({
    username : {
      val : '',
      error : ''
    },
    email : {
      val : '',
      error : ''
    },
    password : {
      val : '',
      error : ''
    }
    ,
    confirm_password : {
      val : '',
      error : ''
    }
  })
  let hasError = ref(false)
  let router = useRouter();
  
  let user = JSON.parse(localStorage.getItem('login_user'))
  if(user) router.push({ name : 'welcome'})
  
  const registerHandler = () => {
    validateForm()
    if(!hasError.value){
      const { username, email, password } = userCredentials
      const userData = {
        username : username.val,
        email : email.val,
        password : password.val
      }
      let users = JSON.parse(localStorage.getItem('users')) || []
      users.push(userData)
      localStorage.setItem('users',JSON.stringify(users))
      router.push({ name : 'login' })

    }
  }
  let validateForm = () => {
    let users = JSON.parse(localStorage.getItem('users'))
    let found = users && users.find((user) => user.email == userCredentials.email.val )
    let error = 0
    if(userCredentials.username.val == ""){
      userCredentials.username.error = 'Username is required!'
      error++
    }else{
      userCredentials.username.error = ""
    }
    if(userCredentials.email.val == ""){
      userCredentials.email.error = 'Email is required!'
      error++
    }else{
      userCredentials.email.error = ""
    }
    if(found){
      userCredentials.email.error = 'Email is taken already!'
      error++
    }else{
      userCredentials.email.error = ""
    }
    if(userCredentials.password.val == ""){
      userCredentials.password.error = 'Password is required!'
      error++
    }else{
      userCredentials.password.error = ""
    }
    if(userCredentials.password.val.length < 3){
      userCredentials.password.error = 'Password minimum length 3!'
      error++
    }else{
      userCredentials.password.error = ""
    }
    if(userCredentials.confirm_password.val !== userCredentials.password.val){
      userCredentials.confirm_password.error = 'Password does not match!'
      error++
    }else{
      userCredentials.confirm_password.error = ""
    }
    hasError.value = (error > 0) ? true : false
  }

</script>
<template>
  <div class="flex items-center justify-center h-screen">
    <div class="w-full max-w-md p-4 bg-blue-100 rounded-md shadow-md shadow-gray-800 shadow-2xl">
      <h2 class="text-2xl font-semibold mb-4 text-center">Register</h2>
      <form @submit.prevent="registerHandler()">
        <div class="mb-4">
          <label for="username" class="block font-semibold text-gray-700">Username <span class="text-red-400" v-if="userCredentials.username.error !=''">{{userCredentials.username.error}}</span></label>
          <input type="text" id="username" name="username" class="w-full px-4 py-2 border rounded-md focus:outline-none focus:ring focus:border-blue-300" v-model="userCredentials.username.val" required>
        </div>
        <div class="mb-4">
          <label for="email" class="block font-semibold text-gray-700">Email <span class="text-red-400" v-if="userCredentials.email.error !=''">{{userCredentials.email.error}}</span></label>
          <input type="email" id="email" name="email" class="w-full px-4 py-2 border rounded-md focus:outline-none focus:ring focus:border-blue-300" v-model="userCredentials.email.val" required>
        </div>
        <div class="mb-4">
          <label for="password" class="block font-semibold text-gray-700">Password <span class="text-red-400" v-if="userCredentials.password.error !=''">{{userCredentials.password.error}}</span></label>
          <input type="password" id="password" name="password" class="w-full px-4 py-2 border rounded-md focus:outline-none focus:ring focus:border-blue-300" v-model="userCredentials.password.val" required>
        </div>
        <div class="mb-4">
          <label for="confirm_password" class="block font-semibold text-gray-700">Confirm Password <span class="text-red-400" v-if="userCredentials.confirm_password.error !=''">{{userCredentials.confirm_password.error}}</span></label>
          <input type="password" id="confirm_password" name="confirm_password" class="w-full px-4 py-2 border rounded-md focus:outline-none focus:ring focus:border-blue-300" v-model="userCredentials.confirm_password.val" required>
        </div>
        <div class="flex items-center justify-between">
          <button type="submit" class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring focus:border-blue-300">Register</button>
          <RouterLink :to="{name:'login', params:{}}">Login</RouterLink>
        </div>
      </form>
    </div>
  </div>
</template>

<style>

</style>
