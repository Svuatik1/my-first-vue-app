<script setup></script>

<template>
  <div class="app">
    <h1>Page with posts</h1>
    <div class="app_btns">
      <my-button @click="showDialog">Create post</my-button>
      <MySelectVue v-model="selectedSort" :options="sortOptions" />
    </div>
    <MyDialog v-model:show="dialogVisible">
      <PostFormVue @create="createPost" />
    </MyDialog>
    <PostListVue
      v-if="!this.isPostLoading"
      :posts="posts"
      @remove="deletePost"
    />
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import PostFormVue from "./components/PostForm.vue";
import PostListVue from "./components/PostList.vue";
import MyDialog from "./components/UI/MyDialog.vue";
import MySelectVue from "./components/UI/MySelect.vue";
import axios from "axios";
export default {
  components: {
    PostFormVue,
    PostListVue,
    MyDialog,
    MySelectVue,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: "",
      sortOptions: [
        { value: "title", name: "from title" },
        { value: "body", name: "from body" },
      ],
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
        this.isPostLoading = true;
        setTimeout(async () => {
          const response = await axios.get(
            "https://jsonplaceholder.typicode.com/posts?_limit=10"
          );
          this.posts = response.data;
          this.isPostLoading = false;
        }, 1000);
      } catch (e) {
        alert("Error");
      }
    },
  },
  mounted() {
    this.fetchPosts();
  },
  watch: {
    selectedSort(newValue) {
      this.posts.sort((post1, post2) => {
        return post1[newValue]?.localeCompare(post2[newValue]);
      });
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
.app_btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
</style>
