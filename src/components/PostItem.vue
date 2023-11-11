<template>
  <div class="post">
    <div>Id - {{ post.id }}</div>
    <div><strong>Название:</strong>{{ post.title }}</div>
    <div><strong>Описание:</strong>{{ post.body }}</div>
    <button @click="$emit('remove', post)">Удалить</button>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.post {
  margin: 0px 0px 15px 0px;
  padding: 10px;
  border: solid 2px #000;
  cursor: pointer;
}
</style>
