<template>
  <div v-if="posts.length > 0">
    <h3>Список Постов</h3>
    <TransitionGroup name="user-list">
      <post-item
      :key="post.id"
        v-for="post in posts"
        :post="post"
        @remove="$emit('remove', post)"
      />
    </TransitionGroup>
  </div>
  <h2 v-else>Нет постов</h2>
</template>

<script>
import PostItem from "./PostItem.vue";
export default {
  props: {
    posts: {
      type: Array,
      required: true,
    },
  },
  components: {
    PostItem,
  },
};
</script>

<style scoped>
.user-list-item {
  display: inline-block;
  margin-right: 10px;
}

.user-list-enter-active,
.user-list-leave-active {
  transition: all 0.4s ease;
}

.user-list-enter-from,
.user-list-leave-to {
  opacity: 0;
  transform: translateX(25%);
}
.user-list-move {
  transition: transform 0.4s ease;
}
</style>
