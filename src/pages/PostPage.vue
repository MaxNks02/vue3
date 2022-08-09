<template>
  <div>
    <h1>Page with posts</h1>

    <my-input
        v-focus
        v-model="searchQuery"
        placeholder="search...">
    </my-input>

    <div class="app_btns">
      <my-button
          @click="showDialog"
      >create posts
      </my-button>

      <div class="app_pagination">
        <my-select
            style="margin-right: 10px"
            v-model="pagination"
            :options="paginationOptions">
        </my-select>

        <my-select
            v-model="selectedSort"
            :options="sortOptions">
        </my-select>
      </div>
    </div>


    <dialog-modal v-model:show="dialogVisible">
      <post-form @create="createPost"/>
    </dialog-modal>

    <post-list
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!isPostLoading"
    />

    <div v-else>
      loading....
    </div>

    <div class="page__wrapper" v-if="pagination==='pagination'">
      <div
          v-for="pageNumber in totalPages"
          :key="pageNumber"
          class="page"
          :class="{
            'current-page' : page===pageNumber
          }"
          @click="changePage(pageNumber)"
      >{{ pageNumber }}
      </div>
    </div>

    <div v-else-if="pagination==='scroll'" v-intersection="loadMorePosts"></div>

  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import DialogModal from "@/components/UI/DialogModal";
import MyButton from "@/components/UI/MyButton";
import axios from 'axios'
import MySelect from "@/components/UI/MySelect";
import MyInput from "@/components/UI/MyInput";

export default {
  components: {MyInput, MySelect, MyButton, DialogModal, PostList, PostForm},
  data: () => ({

    posts: [],
    dialogVisible: false,
    isPostLoading: false,
    selectedSort: '',
    searchQuery: '',
    page: 1,
    limit: 10,
    totalPages: 0,
    pagination: 'pagination',
    paginationOptions: [
      {value: 'pagination', name: 'by pagination'},
      {value: 'scroll', name: 'by scroll'},
    ],
    sortOptions: [
      {value: 'title', name: 'by title'},
      {value: 'body', name: 'by description'},
    ],

  }),
  methods: {
    createPost(post) {
      this.posts.push(post)
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    changePage(pageNumber) {
      this.page = pageNumber
      this.fetchPosts()
    },
    async fetchPosts() {
      try {
        this.isPostLoading = true
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
      } catch (e) {
        alert('error')
      } finally {
        this.isPostLoading = false;
      }
    },

    async loadMorePosts() {
      try {
        this.page += 1;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data];
      } catch (e) {
        alert('Ошибка')
      }
    }
  },
  mounted() {
    this.fetchPosts()
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },


    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },

  watch: {
    // page() {
    //   this.fetchPosts()
    // },

  }


}
</script>

<style>


.app_btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}

.page__wrapper {
  display: flex;
  margin-top: 20px;
  justify-content: center;

}

.page {
  border: 1px solid black;
  padding: 10px;
  margin: 2px;
}

.current-page {
  border: 3px solid teal;
}

.app_pagination {
  height: 36.53px;
  display: flex;
}


</style>