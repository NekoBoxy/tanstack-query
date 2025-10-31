<script setup lang="ts">
import { useQuery } from "@tanstack/vue-query";

export interface Post {
  userId: number;
  id: number;
  title: string;
  body: string;
}

const props = defineProps({
  isVisited: {
    type: Function,
    required: true,
  },
});

const fetcher = async (): Promise<Post[]> =>
  await fetch("https://jsonplaceholder.typicode.com/posts").then((response) =>
    response.json()
  );

const { isPending, isError, isFetching, data, error, refetch } = useQuery({
  queryKey: ["posts"],
  queryFn: fetcher,
});
</script>

<template>
  <h1>Posts</h1>
  <div v-if="isPending">Loading...</div>
  <div v-else-if="isError">An error has occurred: {{ error }}</div>
  <div v-else-if="data">
    <ul>
      <li v-for="item in data" :key="item.id">
        <a
          @click="$emit('setPostId', item.id)"
          href="#"
          :class="{ visited: isVisited(item.id) }"
          >{{ item.title }}</a
        >
      </li>
    </ul>
  </div>
</template>

<style scoped>
.visited {
  font-weight: bold;
  color: green;
}
</style>
