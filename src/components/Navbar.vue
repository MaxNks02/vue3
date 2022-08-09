<template>
  <div class="navbar">
    <div @click="$router.push('/')">App</div>
    <div class="navbar__btns">
      <my-button @click="$router.push('/posts')">Posts</my-button>
      <my-button style="margin-left: 15px" @click="$router.push('/about')">About</my-button>
      <my-button style="margin-left: 15px" @click="$router.push('/store')">Store</my-button>
      <my-button style="margin-left: 15px" @click="$router.push('/composition')">CompositionApi</my-button>
    </div>
  </div>
</template>

<script>
import MyButton from "@/components/UI/MyButton";

export default {
  name: "Navbar",
  components: {
    MyButton
  }
}
</script>

<style scoped>
.navbar {
  height: 45px;
  background: lightblue;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  padding: 0 30px;
  margin-top: 20px;

}

.navbar__btns {
  margin-left: auto;
}

</style>