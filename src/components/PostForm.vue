<template>
  <form class="form" @submit.prevent>
    <input v-model="post.title" type="text" placeholder="Название" />
    <input v-model="post.body" type="text" placeholder="Описание" />
    <button @click="createPost">Создать</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: "",
        body: "",
      },
    };
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit("create", this.post);
      this.post = {
        title: "",
        body: "",
      };
    },
  },
  watch: {
    post: {
      handler(newValue) {
        console.log(newValue);
      },
      deep: true, // Отслеживать все изменения
    },
  },
};
</script>

<style scoped>
.form {
  margin: 0px 0px 30px 0px;
}
</style>
