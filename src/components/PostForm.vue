<template>
  <form @submit.prevent>
    <h4>create app</h4>
    <my-input
        v-focus
        v-model="post.title"
        type="text"
        placeholder="name"
    />
    <my-input
        v-model="post.body"
        type="text"
        placeholder="description"
    />
    <my-button
        style="margin-top: 5px"
        @click="createPost"
    >create
    </my-button>

  </form>
</template>

<script>
import MyButton from "@/components/UI/MyButton";
import MyInput from "@/components/UI/MyInput";

export default {
  components: {MyInput, MyButton},
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now()
      this.$emit('create', this.post)
      this.post = {
        title: '',
        body: ''
      }
    }
  }
}
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}


</style>