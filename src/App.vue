<template>
  <div class="scrolling-component" ref="scrollComponent">
    <post-component v-for="(post, index) in posts" :post="post" :key="index" />
  </div>
</template>

<script>
import PostComponent from "./components/PostComponent";
import { ref, onMounted, onUnmounted } from "vue";
import getPosts from "./utils/getPosts";
export default {
  name: "App",
  components: {
    PostComponent,
  },
  setup() {
    const posts = ref(getPosts(10));
    const scrollComponent = ref(null);

    const loadMorePosts = () => {
      let newPosts = getPosts(10);
      posts.value.push(...newPosts);
    };
    onMounted(() => {
      window.addEventListener("scroll", handleScroll);
    });
    onUnmounted(() => {
      window.removeEventListener("scroll", handleScroll);
    });
    const handleScroll = (e) => {
      let element = scrollComponent.value;
      if (element.getBoundingClientRect().bottom < window.innerHeight) {
        loadMorePosts();
      }
    };

    return {
      posts,
      scrollComponent,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
