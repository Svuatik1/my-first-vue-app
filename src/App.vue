<script setup></script>

<template>
  <div class="app">
    <PostFormVue @create="createPost" />
    <PostListVue :posts="posts" />
  </div>
</template>

<script>
import PostFormVue from "./components/PostForm.vue";
import PostListVue from "./components/PostList.vue";
export default {
  components: {
    PostFormVue,
    PostListVue,
  },
  data() {
    return {
      posts: [
        { id: 1, title: "JavaScript", body: "Description JS" },
        { id: 2, title: "HTML", body: "Description HTML" },
        { id: 3, title: "CSS", body: "Description Css" },
        { id: 4, title: "ReactJS", body: "Description REACT" },
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
.app {
  padding: 20px;
}
</style>
