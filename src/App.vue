<template>
  <div class="app">
    <div>количество лайков: <strong>{{ likes }}</strong></div>
    <div>количество дизлайков: <strong>{{ dislikes }}</strong></div>
    <button @click="addLike">like</button>
    <button @click="addDisLike">dislike</button>

    <h1>Страница с постами</h1>

    <my-button @click="fetchPosts" style="margin: 15px 0">Получить посты</my-button>
    <my-button @click="showDialog" style="margin: 15px 0">Cоздать пост</my-button>
    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>

    <post-list
        :posts="posts"
        @remove="removePost"
        v-if="!isPostLoading"
    />
    <div v-else>Идет загрузка</div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList"
import MyButton from "@/components/UI/MyButton";
import axios from "axios"

export default {
  components: {
    MyButton,
    PostList, PostForm
  },
  data() {
    return {
      likes: 5,
      dislikes: 0,
      posts: [],
      dialogVisible: false,
      isPostLoading: false
    }
  },
  methods: {
    addLike() {
      this.likes += 1
    },
    addDisLike() {
      this.dislikes += 1
    },
    createPost(post) {
      this.posts.push(post)
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    async fetchPosts() {
      try {
        this.isPostLoading = true
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts?_limit=10")
        this.posts = response.data
      } catch (e) {
        alert("ошибка")
      } finally {
        this.isPostLoading = false
      }
    },
  },
  mounted() {
    this.fetchPosts()
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
</style>
