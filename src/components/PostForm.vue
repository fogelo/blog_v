<template>
  <form class="form" @submit.prevent>
    <h4>Создание поста</h4>
    <my-input v-model="post.title" type="text" placeholder="Название"/>
    <my-input v-model="post.body" type="text" placeholder="Описание"/>
    <my-button @click="createPost">Создать</my-button>
  </form>
</template>

<script>
import {uuid} from "vue-uuid"

export default {
  data() {
    return {
      post: {
        title: "",
        body: ""
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = uuid.v1()
      this.$emit("create", this.post)
      this.post = {
        title: "",
        body: ""
      }

      console.log(this.post)
    }
  }
}
</script>

<style scoped>
.form {
  margin: 15px 0;
  display: flex;
  flex-direction: column;
}


</style>