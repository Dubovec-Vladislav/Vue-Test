<template>
  <my-dialog v-model:show="dialogVisible">
    <post-form @create="createPost" />
  </my-dialog>

  <my-select v-model="selectedSort" :options="sortOptions" />
  <input type="text" v-model.trim="searchQuery" placeholder="Поиск..." />

  <button @click="showDialog">Создать Пост</button>

  <post-list
    v-if="!isPostsLoading"
    :posts="sortedAndSearchPosts"
    @remove="removePost"
  />
  <div v-else>Загрузка...</div>

  <!-- <div v-if="totalPages > 0"> -->
  <div class="pages">
    <div
      :key="pageNumber"
      class="page"
      :class="{
        'curren-page': page === pageNumber,
      }"
      @click="changePage(pageNumber)"
      v-for="pageNumber in totalPages"
    >
      {{ pageNumber }}
    </div>
  </div>
  <!-- </div> -->
</template>

<script>
import PostList from "./components/PostList";
import PostForm from "./components/PostForm";
import MyDialog from "./components/UI/MyDialog";
import MySelect from "./components/UI/MySelect";

export default {
  components: {
    PostList,
    PostForm,
    MyDialog,
    MySelect,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      sortOptions: [
        { value: "title", name: "По значению" },
        { value: "body", name: "По содержимому" },
        { value: "id", name: "По id" },
      ],
      selectedSort: "title",
      searchQuery: "",

      page: 1,
      limit: 10,
      totalPages: 0,

      isPostsLoading: false,
    };
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await fetch(
          `https://jsonplaceholder.typicode.com/posts?_limit=${this.limit}&_page=${this.page}&_sort=${this.selectedSort}`
        );
        const data = await response.json();
        this.posts = data;
        this.isPostsLoading = false;
        this.totalPages = Math.ceil(
          response.headers.get("x-total-count") / this.limit
        );
      } catch (e) {
        console.log("Error");
      }
    },
    changePage(pageNumber) {
      this.page = pageNumber;
    },
  },
  computed: {
    sortedAndSearchPosts() {
      return this.posts.filter((post) => post.title.includes(this.searchQuery));
    },
  },
  mounted() {
    this.fetchPosts();
  },
  watch: {
    page() {
      this.fetchPosts();
    },
    selectedSort() {
      this.fetchPosts();
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  padding: 20px 60px;
}

.pages {
  display: flex;
}

.page {
  display: flex;
  color: #000;
  width: 30px;
  height: 30px;
  margin: 0px 10px 0px 0px;
  border: solid 1px #000;
  border-radius: 50%;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: 0.4s ease;
}

.page:hover {
  color: #fff;
  background-color: #000;
}

.curren-page {
  background-color: #000;
  color: #fff;
}
</style>
