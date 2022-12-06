<template>
  <div>
    <br><br>
    <Menu/>
    <div v-if="success">
      <h3 style="text-align: center;">Not logged in </h3>
      <p style="text-align: center;"><button class="btn" @click="logout()">Go back to Products</button></p>
    </div>
    <div v-else>
      <h1 style="text-align: center;">Logout ?</h1>
      <p style="text-align: center;">
        <button class="btn" @click="cancel()" >Cancel</button>
        &nbsp;&nbsp;
        <button class="btn" @click="logout()" >Logout</button>
      </p>
    </div>
    <Footer/>
  </div>
</template>

<script>

import Menu from "@/components/Header";
import Footer from "@/components/Footer";
export default {
  components: {Footer, Menu},
  data() {
    return {
      success: false,
      user: {},
    }
  },


  methods: {
    logout: function() {

      this.success = true

      if (this.userLoggedIn) {
        // destroy session
        this.logoutUser(this.user.session_id)
      } else {
        // No user is signed in.
        this.$router.push('/')
      }

    },

    async logoutUser(session_id) {
      if ( await this.$store.dispatch('user/logoutUser', session_id) ) {
        // using local storage to pass information between views
        localStorage.setItem('message', 'Bye, come back soon!');
        await this.$router.push('/message/6')
      }
    },

    cancel: function(){
      this.$router.push('/')
    },

    getUser() {
      this.user = this.$store.getters['user/getUser']
    },

  },

  created: function () {
    this.success = !this.userLoggedIn;
  },

  computed: {
    userLoggedIn: function () {
      this.getUser()
      for (var i in this.user) return true
      return false
    },
  }

}
</script>

<style scoped>
</style>