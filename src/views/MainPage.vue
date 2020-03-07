<template>
  <div>
    <div class="main list-container contents">
      <h1 class="page-header">Today I Learned</h1>

      <LoadingSpinner v-if="isLoading"></LoadingSpinner>
      <ul v-else>
        <PostListItem
          v-for="postItem in postItems"
          :key="postItem._id"
          :postItem="postItem"
        ></PostListItem>
      </ul>
    </div>
  </div>
</template>

<script>
import { fetchPosts } from '@/api/index';
import PostListItem from '@/components/posts/PostListItem';
import LoadingSpinner from '@/components/common/LoadingSpinner';

export default {
  components: {
    PostListItem,
    LoadingSpinner,
  },
  data() {
    return {
      postItems: [],
      isLoading: false,
    };
  },
  methods: {
    async fetchNotes() {
      try {
        this.isLoading = true;
        const response = await fetchPosts();
        //console.log(response);
        this.postItems = response.data.posts;
        this.isLoading = false;
      } catch (error) {
        console.log(error);
      }
    },
  },
  created() {
    this.fetchNotes();
  },
};
</script>

<style></style>
