<template>
  <div class="app">
    <div>количество лайков: <strong>{{ likes }}</strong></div>
    <div>количество дизлайков: <strong>{{ dislikes }}</strong></div>
    <button @click="addLike">like</button>
    <button @click="addDisLike">dislike</button>

    <h1>Страница с постами</h1>
    <my-input v-model="searchQuery" placeholder="Поиск..."/>
    <div class="app__btns">
      <my-button @click="showDialog" style="margin: 15px 0">Cоздать пост</my-button>
      <my-select
          v-model="selectedSort"
          :options="sortOption"
      />
    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>

    <post-list
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!isPostLoading"
    />
    <div v-else>Идет загрузка</div>
    <div class="page__wrapper">
      <div v-for="pageNumber in totalPages"
           :key="pageNumber"
           class="page"
           :class="{
             'current-page':page===pageNumber
           }"
           @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div>
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
      isPostLoading: false,
      searchQuery: "",
      selectedSort: "",
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOption: [
        {value: "title", name: "по названию"},
        {value: "body", name: "по содержимому"},
      ]
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
    changePage(pageNumber) {
      this.page = pageNumber
      this.fetchPosts()
    },
    async fetchPosts() {
      try {
        this.isPostLoading = true
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        })
        this.totalPages = Math.ceil(response.headers["x-total-count"] / this.limit)
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
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },
    sortedAndSearchedPosts() {
      return [...this.sortedPosts].filter(post => post.title.includes(this.searchQuery))
    }
  },

  watch: {
    // selectedSort(newValue) {
    //   this.posts.sort((post1, post2) => {
    //     return post1[newValue]?.localeCompare(post2[newValue])
    //   })
    // }
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

.app__btns {
  display: flex;
  justify-content: space-between;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid;
  padding: 10px;
}

.current-page {
  border: 2px solid teal;
  background: teal;
}
</style>
