<template>
  <div class="app">
    <div>количество лайков: <strong>{{ likes }}</strong></div>
    <div>количество дизлайков: <strong>{{ dislikes }}</strong></div>
    <button @click="addLike">like</button>
    <button @click="addDisLike">dislike</button>
    <form class="form">
      <h4>Создание поста</h4>
      <input v-bind:value="title" @input="title = $event.target.value" class="input" type="text" placeholder="Название">
      <input v-bind:value="body" @input="body = $event.target.value" class="input" type="text" placeholder="Описание">
      <button class="btn">Создать</button>
    </form>
    <div class="post" v-for="post in posts" v-bind:key="post.id">
      <div><strong>Название: </strong> {{ post.title }}</div>
      <div><strong>Описание: </strong> {{ post.body }}</div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      likes: 5,
      dislikes: 0,
      posts: [
        {id: 1, title: "javascript", body: "описание поста 1"},
        {id: 2, title: "javascript", body: "описание поста 2"},
        {id: 3, title: "javascript", body: "описание поста 3"},
      ],
      title: "",
      body: ""
    }
  },
  methods: {
    addLike() {
      this.likes += 1
    },
    addDisLike() {
      this.dislikes += 1
    },
    inputValue(event) {
      this.title = event.target.value
    }
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.app {
  padding: 10px;
}

.post {
  padding: 10px;
  margin-bottom: 15px;
  border: 2px solid teal;
}

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
