<template>
  <form class="form" @submit.prevent>
    <h4>Создание поста</h4>
    <input v-model="post.title" class="input" type="text" placeholder="Название">
    <input v-model="post.body" class="input" type="text" placeholder="Описание">
    <button class="btn" @click="createPost">Создать</button>
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

.input {
  width: 100%;
  margin-bottom: 15px;
  border: 1px solid teal;
  padding: 10px 15px;
}

.btn {
  align-self: flex-end;
  padding: 10px 15px;
  background: none;
  color: teal;
  border: 1px solid teal;
}
</style>