<template>
<div id="bar">
  <nav class="navbar navbar-fixed-top navbar-light bg-light">
    <div class="container-fluid">
      <div class="navbar-header">
        <div class="navbar-brand">The Shop</div>
      </div>
      <ul  class="nav navbar-nav navbar-left">
        <li class="active"><router-link to="/">Home</router-link></li>
      </ul>
      <ul  class="nav navbar-nav navbar-right" style="width: 10%">
        <li  v-if ="!userLoggedIn" class="active"><router-link to="/Basket"><img src="https://cdn-icons-png.flaticon.com/512/60/60992.png" width="15%" height="15%" alt="Shopping cart free icon" title="Shopping cart free icon">{{basketCount}}  Basket </router-link></li>
        <li  v-if ="userLoggedIn" class="active"><router-link to="/Basket"><img src="https://cdn-icons-png.flaticon.com/512/60/60992.png" width="15%" height="15%" alt="Shopping cart free icon" title="Shopping cart free icon">{{basketCount}}  {{user.name}}'s Basket </router-link></li>

      </ul>
      <ul class="nav navbar-nav navbar-right">
        <li class="active"><router-link to="/products">Products</router-link></li>
      </ul>
      <ul v-if="userLoggedIn" class="nav navbar-nav navbar-right">
        <li class="active"><router-link to="/myorders">{{user.name}}'s Orders</router-link></li>
      </ul>
      <ul  v-if="userLoggedIn" class="nav navbar-nav navbar-right">
        <li  class="active"><router-link to="/Logout">Logout</router-link></li>
      </ul>
      <ul v-if="!userLoggedIn"  class="nav navbar-nav navbar-right">
        <li class="active"><router-link to="/Login">Login</router-link></li>
      </ul>
      <ul v-if="!userLoggedIn" class="nav navbar-nav navbar-right">
        <li class="active"><router-link to="/Register">Register</router-link></li>
      </ul>
    </div>
  </nav>
</div>
</template>

<script>

export default {
  data() {
    return {
      user: {
        id: '',
        name: '',
        email: '',
        session_id: ''
      },
    }
  },
  mounted() {
    this.getUser();
  },
  methods: {
    getUser() {
      this.user = this.$store.getters['user/getUser']
    },

  },
  computed: {
    userLoggedIn: function () {
      this.getUser()
      for(let i in this.user) return true
      return false
    },
    basketCount: function (){
      return this.$store.getters['basket/getProducts'].length
    }
  }

}

</script>
<style>
  .navbar{
    height: 1%;
    background: darkgray;
    position: fixed;
  }
</style>