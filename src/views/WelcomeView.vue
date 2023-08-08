<script setup>
    import { reactive } from 'vue';
    import { useRouter } from 'vue-router';
    

    let router = useRouter()
    let userToShow = reactive({})


    let user = JSON.parse(localStorage.getItem('login_user'))
    let users = JSON.parse(localStorage.getItem('users'))
    
    let found = users.length > 0 ? users.find(u => u.email === user.email) : []
    userToShow = found
    if(!found) router.push({name : 'login'})

    const logOutHandler = () => {
        localStorage.removeItem('login_user')
        router.push({ name : 'login' })
    }

    
</script>

<template>
    <div class="flex items-center justify-center h-screen">
        <div class="w-full max-w-md p-4 bg-blue-100 rounded-md shadow-md">
            <h2 class="text-2xl font-semibold mb-4">Hello {{userToShow?.username}}, Welcome to our website!</h2>
            <p class="text-gray-700">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed aliquet est sed nibh aliquet, ut malesuada turpis lobortis.</p>
            <button @click="logOutHandler()" class="block mt-4 text-black-500 hover:underline bg-blue-300 py-2 px-4 rounded-md">Logout</button>
        </div>
    </div>
</template>
