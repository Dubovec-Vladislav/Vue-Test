<template>
  <div class="app"></div>
  <post-form @create="createPost" />
  <post-list :posts="posts" />
</template>

<script>
import PostList from "./components/PostList";
import PostForm from "./components/PostForm";
export default {
  components: {
    PostList,
    PostForm,
  },
  data() {
    return {
      posts: [
        { id: 1, title: "Title 1", body: "Body 1" },
        { id: 2, title: "Title 2", body: "Body 2" },
        { id: 3, title: "Title 3", body: "Body 3" },
        { id: 4, title: "Title 4", body: "Body 4" },
        { id: 5, title: "Title 5", body: "Body 5" },
      ],
    };
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
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
  padding: 20px;
}

.form {
  margin: 0px 0px 20px 0px;
}
</style>
