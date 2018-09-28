<template>
  <div id="app">
  	<Header></Header>
    <div class="main">
      <router-view name="slide"></router-view>
      <router-view name="main"></router-view>
    </div>
  </div>
</template>

<script>
	import Header from "./components/Header"
	import PostList from "./components/Postlist"
  import Article from "./components/Article"
  import Slidebar from "./components/Slidebar"

export default {
  name: 'App',
  components:{
  	Header,
  	PostList,
    Article,
    Slidebar
  }
}
</script>

<style>
.main{
  width: 80%;
  margin: 0 auto;
}
body{
  background-color: #e1e1e1;
}
</style>
