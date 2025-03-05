<script setup>
import { ref, computed } from "vue";
import PostCard from "@/components/PostCard.vue";

//let text = "hello";
const text = ref("");
const trimmmedText = computed(() => text.value.trim());

const posts = ref([]);

const postsRevers = computed(() =>  posts.value.toSorted((a, b) => b.createdAt - a.createdAt));



function addPost() {
  const newPost = {
    id: Math.random().toString(36).substring(2),
    content: trimmmedText.value,
    createdAt: new Date(),
    author: {
      username: "goat",
      avatarUrl: "https://media1.tenor.com/m/a5RGfluwSOgAAAAd/sylvian-delhoumi.gif"
    },
    like : 0
  };
  posts.value.push(newPost)
  
  text.value = "";
}

function deletePost(id){
  posts.value = posts.value.filter((post) => post.id != id)
}

function likePost(id){ 
  var post = posts.value.find((p)=>{
    return p.id == id
  })
  if(post != null){
    post.like++;
  }
}
</script>


<template>
  <main>
    <div class="container">
      <form class="card" @submit.prevent="addPost">
        <textarea name="post" id="post" placeholder="Quoi de neuf ?" v-model="text" ></textarea>
        <button type="submit" :disabled="!trimmmedText">Publier</button>
      </form>

      <h2 v-if="!posts.length">Aucun posts pour le moment</h2>

      <PostCard v-for="(post, index) in postsRevers" :key="index" class="card" :post="post" @delete="deletePost" @like="likePost(id)"/>

    </div>
  </main>
</template>