<script setup>
import { useRouter } from 'vue-router'

const router = useRouter()

const Logout = async () => {
    const response = await fetch('http://127.0.0.1:3333/logout', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            session_token: localStorage.getItem('token')
        })
    }).then(response => response.json())

    if (response.success) {
        localStorage.removeItem('token')
        router.push('/login')
    } else {
        alert(response.message)
    }
}
</script>

<template>
    <main>
        <h1>This is Home Page</h1>

        <button @click="Logout">Logout</button>
    </main>
</template>

<style scoped>
main {
    padding: 1.5rem;
}

h1 {
    margin-bottom: 1rem;
}
</style>