<script setup>
import { ref, computed } from "vue";
import PostCard from "@/components/PostCard.vue";

//let text = "hello";
const text = ref("");
const trimmmedText = computed(() => text.value.trim());

const posts = ref([]);

const postsRevers = computed(() =>  posts.value.toSorted((a, b) => b.createdAt - a.createdAt));


/*
function addPost() {
  const newPost = {
    id: Math.random().toString(36).substring(2),
    content: trimmmedText.value,
    createdAt: new Date(),
    author: {
      username: "goat",
      avatarUrl: "https://media1.tenor.com/m/a5RGfluwSOgAAAAd/sylvian-delhoumi.gif"
    },
    like : false
  };
  posts.value.push(newPost)
  
  text.value = "";
}*/
function addPost(){
  const user = JSON.parse(localStorage.getItem("user"))
  const token = user.token
  fetch("https://posts-crud-api.vercel.app/posts",{
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Authorization": `Bearer ${ token }`
    },
    body: JSON.stringify({content: trimmmedText.value})
  })
  .then(response => response.json())
  .then((data) => {
    apiPosts.value.unshift({
      id: data.id,
      content: data.content,
      createdAt: data.createdAt,
      author: {
        id: user.user.id,
        username: username.user.username,
        avatarUrl: user.user.avatarUrl
      }
    })
    text.value = ""
  })
}

function deletePost(id){
  posts.value = posts.value.filter((post) => post.id != id)
}

function likePost(id){ 
  console.log(id);
  
  const post = posts.value.find((p)=> p.id === id )
  post.like = !post.like;
}

const apiPosts = ref([]);
const loading = ref(false);
function fetchPosts(){
  loading.value = true;
    const result = fetch("https://posts-crud-api.vercel.app/posts");
    result.then((response) => response.json())
    .then((data) => {
        apiPosts.value = data;
        loading.value = false;
    });
}

fetchPosts();
</script>


<template>
  <main>
    <div class="container">
      <form class="card" @submit.prevent="addPost">
        <textarea name="post" id="post" placeholder="Quoi de neuf ?" v-model="text" ></textarea>
        <button type="submit" :disabled="!trimmmedText">Publier</button>
      </form>
      
      <h2 v-if="loading">Chargement...</h2>
      <h2 v-else-if="!apiPosts.length">Aucun posts pour le moment</h2>

      <PostCard v-for="(post, index) in apiPosts" :key="index" class="card" :post="post" @delete="deletePost" @like="likePost"/>

    </div>
  </main>
</template>