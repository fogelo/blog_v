<template>
  <div v-if="posts.length > 0">
    <post-item v-for="post in posts" v-bind:key="post.id" :post="post" @remove="$emit('remove', post)"/>
  </div>
  <h2 v-else style="color: red">
    Список постов пуст
  </h2>
</template>

<script>

import PostItem from "@/components/PostItem";

export default {
  components: {PostItem},
  props: {
    posts: {
      type: Array,
      required: true
    }
  }

}
</script>

<style scoped>

</style>