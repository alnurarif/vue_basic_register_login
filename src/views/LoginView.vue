<script setup>
import { reactive, ref } from 'vue'
import { useRouter, RouterLink } from 'vue-router';

const userCredentials = reactive({
  email : '',
  password : ''
})
let errorMessage = ref('')
let router = useRouter();
let user = JSON.parse(localStorage.getItem('login_user'))
if(user) router.push({ name : 'welcome'})
const loginHandler = () => {
    let users = JSON.parse(localStorage.getItem('users')) || []
    let found = users.find(user => userCredentials.email === user.email && userCredentials.password === user.password)
    if(found){
      localStorage.setItem('login_user', JSON.stringify(userCredentials))
      router.push({ name : 'welcome' })
    }else{
      errorMessage.value = "Email or Password doesn't match"
    }

  }
</script>

<template>
  <div class="flex items-center justify-center h-screen">
    <div class="w-full max-w-md p-4 bg-blue-100 rounded-md shadow-gray-800 shadow-2xl">
      <h2 class="text-2xl font-semibold mb-4 text-center">Login</h2>
      <div class="h-12">
        <p class="text-red-400 text-center" v-if="errorMessage!=''">{{errorMessage}}</p>
        
      </div>
      <form @submit.prevent="loginHandler()">
        <div class="mb-4">
          <label for="email" class="block font-semibold text-gray-700">Email</label>
          <input type="text" id="email" name="email" class="w-full px-4 py-2 border rounded-md focus:outline-none focus:ring focus:border-blue-300" v-model="userCredentials.email" required>
        </div>
        <div class="mb-4">
          <label for="password" class="block font-semibold text-gray-700">Password</label>
          <input type="password" id="password" name="password" class="w-full px-4 py-2 border rounded-md focus:outline-none focus:ring focus:border-blue-300" v-model="userCredentials.password" required>
        </div>
        <div class="flex items-center justify-between">
          <button type="submit" class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring focus:border-blue-300">Login</button>
          <!-- <a href="register.html" class="text-blue-500 hover:underline">Register</a> -->
          <RouterLink :to="{name:'register',params:{}}" class="font-semibold underline">Register</RouterLink>
        </div>
      </form>
    </div>
  </div>
</template>
