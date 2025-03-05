<script setup>
import { ref, computed } from "vue";
//let text = "hello";
const text = ref("");
const trimmmedText = computed(() => text.value.trim());

const posts = ref([]);

const postsRevers = computed(() =>  posts.sort(createdAt));



function addPost() {
  const newPost = {
    id: Math.random().toString(36).substring(2),
    content: trimmmedText.value,
    createdAt: new Date(),
    author: {
      username: "goat",
      avatarUrl: "https://media1.tenor.com/m/a5RGfluwSOgAAAAd/sylvian-delhoumi.gif"
    }
  };
  posts.value.push(newPost)
  
  text.value = "";
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

      <article  v-for="(post, index) in postsRevers" :key="index" class="card">
        <header>
          <img :src="post.author.avatarUrl" alt="avatar" width="36" height="36">
          <a>{{ post.author.username }}</a>
        </header>
        <p>
          {{ post.content }}
        </p>
      </article>
    </div>
  </main>
</template>

<style scoped>
.container {
  height: 100vh;
  margin: 0 auto;
  max-width: 640px;
}
.card {
  background-color: var(--color-bg-secondary);
  border-radius: 10px;
  border: 1px solid var(--color-border);
  margin-bottom: 1rem;
}

form {
  display: flex;
  flex-direction: column;
  margin-top: 1rem;
  padding: 1rem 1.5rem;
  width: 100%;
}
textarea {
  background: none;
  border: none;
  color: var(--color-text-primary);
  flex: 1;
  margin-bottom: 1rem;
  outline: none;
  padding: 0.5rem 0;
  resize: none;
}
button {
  align-self: flex-end;
  background: none;
  border-radius: 10px;
  border: 1px solid var(--color-border);
  color: var(--color-text-primary);
  cursor: pointer;
  font-size: 1rem;
  height: 40px;
  padding: 0 1rem;
}

button:disabled{
  opacity: 0.4;
  cursor: not-allowed;
}

article {
  padding: 1rem;
  overflow: hidden;
}

article p {
  white-space: pre-wrap;
}

article header{
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

article img {
  border-radius: 50%;
}

</style>
