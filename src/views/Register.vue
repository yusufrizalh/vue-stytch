<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const email = ref('')
const password = ref('')
const conf_password = ref('')

const router = useRouter()

const Register = async () => {
    if (!email.value || !password.value || !conf_password.value) {
        return alert('Please fill in all fields!')
    }
    if (password.value !== conf_password.value) {
        return alert('Passwords do not match!')
    }

    const response = await fetch('http://127.0.0.1:3333/register', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({
            email: email.value, password: password.value,
        })
    }).then(response => response.json())

    if (response.success) {
        localStorage.setItem('token', response.token)
        router.push('/')
    } else {
        alert(response.message)
    }
}
</script>

<template>
    <main>
        <header>
            <h1 class="logo">AUTH APP</h1>
            <h2>Register Page</h2>
            <p>
                Login or create an account to start using the
                <strong>Authentication App</strong>
            </p>
        </header>

        <form @submit.prevent="Register">
            <label for="email">
                <span>Enter your email</span>
                <input type="email" placeholder="test@test.com" v-model="email" />
            </label>
            <label for="password">
                <span>Enter your password</span>
                <input type="password" placeholder="************" v-model="password" />
            </label>
            <label for="conf_password">
                <span>Confirm your password</span>
                <input type="password" placeholder="************" v-model="conf_password" />
            </label>
            <input type="submit" value="Register" />
        </form>

        <footer>
            <p>Already have an accout?
                <router-link to="/login">Login here</router-link>
            </p>
        </footer>
    </main>
</template>

<style scoped>
main {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;
    height: 100vh;
    background-color: var(--primary);
    color: #ffffff;
}

header {
    padding: 1.5rem;
}

footer {
    background-color: #ffffff;
    width: 100%;
    color: var(--dark);
    text-align: center;
    padding: 1.5rem;
    padding-bottom: 3rem;
}

h2 {
    font-size: 2.125rem;
    margin-bottom: 1rem;
}

h2~p {
    font-weight: 500;
    font-size: 1rem;
}

form {
    flex: 1 1 0%;
    display: block;
    border-radius: 1.5rem 1.5rem 0 0;
    background-color: #ffffff;
    box-shadow: 0px -4px 12px 4px rgba(0, 0, 0, 0.16);
    color: var(--dark);
    padding: 4rem 1.5rem;
    width: 100%;
}

label {
    display: block;
    margin-bottom: 1.5rem;
}

label span {
    display: block;
    color: var(--gray);
    font-size: 1rem;
    font-weight: 500;
    margin-bottom: 0.5rem;
}

input:not([type="submit"]) {
    display: block;
    width: 100%;
    /* border: 1px solid var(--gray); */
    border-radius: 1rem;
    padding: 1.5rem 1rem;
    font-size: 1.125rem;
    font-weight: 500;
    color: var(--dark);
    background-color: var(--light);
}

input:not([type="submit"]) {
    color: var(--gray);
    font-style: italic;
}

input[type="submit"] {
    display: block;
    width: fit-content;
    margin: 0 auto;
    font-size: 1.5rem;
    font-weight: 700;
    color: #ffffff;
    background-color: var(--primary);
    padding: 1rem;
    border-radius: 1rem;
    cursor: pointer;
    transition: 0.5s ease;
}

input[type="submit"]:hover {
    background-color: var(--primary-dark);
}
</style>