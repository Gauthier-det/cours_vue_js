<script setup>
import { routerKey, useRouter } from 'vue-router';
import { ref, computed } from "vue";
const route = useRouter();

const loading = ref(false);
const username = ref("");
const mail = ref("");
const password = ref("");

const isValid = computed(() =>{
    return !!username.value.trim() && !!mail.value.trim() && !!password.value.trim();
})

function register(){
    /*
    loading.value = true;
    console.log({
        username: username.value,
        mail: mail.value,
        password: mail.value
    })
    setTimeout(()=>{
        route.push("/");
    }, 1000)*/
    fetch("https://posts-crud-api.vercel.app/register", {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify({
            username: username.value,
            email: mail.value,
            password: mail.value
        })
    })
    .then((response) => response.json())
    .then((data) => {
        console.log(data)
        localStorage.setItem("user", JSON.stringify(data));
        route.push("/")
    })
}

</script>

<template>
    <div class="container">
        <form class="card" @submit.prevent="register">
            <label for="username">Nom d'utilisateur :</label>
            <input type="text" name="username" id="username" v-model="username" required>
            <label for="username">Mail :</label>
            <input type="email" name="mail" id="mail" v-model="mail" required>
            <label for="username">Moteeeeeeee de passe</label>
            <input type="password" name="password" id="password" v-model="password" required>
            <input type="submit" value="Submit" :disabled="loading || !isValid">
        </form>
    </div>
</template>

