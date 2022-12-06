<template >
  <div id="app">
    <img alt="Vue logo" width="25%" height="25%" src="@/assets/images/logo.png">
    <h1>Welcome to The Shop</h1>
    <router-link to="/products"><button class="btn" > Explore The Shop </button></router-link>
    <Footer/>
  </div>
</template>

<script>

import Footer from "@/components/Footer";

export default {
  components: {Footer}
}
</script>

<style scoped>
	#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 100px;
	}
</style>