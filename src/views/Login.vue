<template>
  <div>
    <Menu/>
    <br><br>
    <h1 style="text-align: center">Login</h1>
    <form class="form-group" @submit.prevent="handleSubmit" @keydown.enter.exact.prevent="handleSubmit">
      <label>Email</label>
      <input
          class = "form-control"
          type="email"
          placeholder="Please enter a valid email"
          :class="{ 'has-error': submitting && invalidEmail }"
          v-model="user.email"
          @focus="clearStatus"
          ref="first"
          v-autofocus
          @keypress="clearStatus"
      />

      <label>Password</label>
      <input
          class = "form-control"
          type="password"
          :class="{ 'has-error': submitting && invalidPassword }"
          v-model="user.password"
          @focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">
        Please fill out all required fields
      </p>
      <br>
      <h3 style="text-align:center;">
        <button class="btn" @click="cancel()" >Cancel</button>
        <button class="btn" type="submit">Login</button>
      </h3>
      <br><br>
      <h3 style="text-align: center"> Not yet registered? Register
        <router-link to="/register">here</router-link>
      </h3>

    </form>
  </div>
</template>

<script>


import Menu from "@/components/Header";
export default {
  components: {Menu},
  data() {
    return {
      user: {
        id: '',
        name: '',
        email: '',
        session_id: '',
      },
      submitting: false,
      error: false,
    }
  },

  methods: {
    handleSubmit: function () {
      this.submitting = true
      this.clearStatus()

      if (this.invalidEmail || this.invalidPassword) {
        this.error = true
        return
      }

      this.loginUser(this.user)


    },
    clearStatus: function () {
      this.error = false
    },

    async loginUser(user) {
      if (await this.$store.dispatch('user/loginUser', user)) {
        //login valid
        this.userLoggedIn = true;
        await this.$router.push('/message/5')
      } else {
        //login failed
        this.error = true
        this.submitting = false
      }

    },


    cancel() {
      this.$router.push('/')
    },
  },

  computed: {
    invalidPassword: function () {
      return this.user.password === '';
    },

    invalidEmail: function () {
      return this.user.email === '' || this.user.email.search('@') === -1;
    },
  },
  directives: {
    autofocus: {
      inserted(el) {
        el.focus()
      }
    }
  },
  created: function () {
    this.submitting = false
    this.error = false
  }
}
</script>

<style scoped>

</style>