<script setup>
import {TrashIcon, HandThumbUpIcon} from "@heroicons/vue/24/outline";

const props = defineProps({
    post: {
        type: Object,
        required: true
    }
});

const emit = defineEmits(["delete", "like"]);

function deletePost(){
    emit("delete", props.post.id);
    
}

function likePost(){
    emit("like", props.post.id);
    
}
</script>

<template>
    <article class="card">
        <header>
          <img :src="post.author.avatarUrl" alt="avatar" width="36" height="36" class="avatar">
          <a>{{ post.author.username }}</a>
        </header>
        <p>
          {{ post.content }}
        </p>

        <footer>
            <button @click="deletePost" class="btn-icon">
                <TrashIcon />
            </button>
            <p>{{ post.like }}</p>
            <button @click="likePost" class="btn-icon">
                <HandThumbUpIcon />
            </button>
        </footer>

      </article>
</template>