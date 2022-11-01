<script setup></script>

<template>
  <div class="app">
    <h1>Page with posts</h1>
    <my-button @click="showDialog" style="margin: 15px 0px"
      >Create post</my-button
    >
    <MyDialog v-model:show="dialogVisible">
      <PostFormVue @create="createPost" />
    </MyDialog>
    <PostListVue :posts="posts" @remove="deletePost" />
  </div>
</template>

<script>
import PostFormVue from "./components/PostForm.vue";
import PostListVue from "./components/PostList.vue";
import MyDialog from "./components/UI/MyDialog.vue";
import axios from "axios";
export default {
  components: {
    PostFormVue,
    PostListVue,
    MyDialog,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
    };
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    deletePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchPosts() {
      try {
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts?_limit=10"
        );
        this.posts = response.data;
      } catch (e) {
        alert("Error");
      }
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
