<script setup lang="ts">
import { ref } from "vue";
import Post from "./pages/Post.vue";
import Posts from "./pages/Posts.vue";
import { VueQueryDevtools } from "@tanstack/vue-query-devtools";

const visitedPosts = ref(new Set());
const postId = ref(-1);

const isVisited = (id: number) => visitedPosts.value.has(id);

const setPostId = (id: number) => {
  visitedPosts.value.add(id);
  postId.value = id;
};
</script>

<template>
  <Post v-if="postId > -1" :postId="postId" @setPostId="setPostId" />
  <Posts v-else :isVisited="isVisited" @setPostId="setPostId" />
  <VueQueryDevtools initialIsOpen />
</template>

<style scoped></style>
