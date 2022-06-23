<template>
  <div>количество лайков: <strong>{{ likes }}</strong></div>
  <div>количество дизлайков: <strong>{{ dislikes }}</strong></div>
  <button @click="addLike">like</button>
  <button @click="addDisLike">dislike</button>
</template>

<script>

export default {
  data() {
    return {
      likes: 5,
      dislikes: 0
    }
  },
  methods: {
    addLike() {
      this.likes += 1
    },
    addDisLike() {
      this.dislikes += 1
    }
  }
}
</script>

<style>

</style>
