<template>
  <div>
    <Menu/>
    <br>
    <br>
    <form class="form-group" @submit.prevent="handleSubmit" @keydown.enter.exact.prevent="handleSubmit">
      <h1 style="text-align: center">Register</h1>
      <label>Name</label>
      <input
          class="form-control"
          type="text"
          placeholder="Enter name"
          :class="{ 'has-error': submitting && invalidName }"
          v-model="user.name"
          v-autofocus
          @focus="clearStatus"
          @keypress="clearStatus"
      />
      <label>Email</label>
      <input
          class="form-control"
          type="email"
          placeholder="Please enter a valid email"
          :class="{ 'has-error': submitting && invalidEmail }"
          v-model="user.email"
          @focus="clearStatus"
      />

      <label>Password</label>
      <input
          class="form-control"
          type="password"
          placeholder="Please enter a 6 or more characters"
          :class="{ 'has-error': submitting && invalidPassword }"
          v-model="user.password"
          @focus="clearStatus"
      />

      <label>Confirm Password</label>
      <input
          class="form-control"
          type="password"
          :class="{ 'has-error': submitting && invalidRepeatPassword }"
          v-model="passwordConfirmation"
          @focus="clearStatus"
      />
      <p v-if="error && submitting" class="alert alert-danger">
        Please fill out all required fields
      </p>
      <br>
      <h3 style="text-align:center;">
        <button class="btn" @click="cancel()">Cancel</button>
        <button class="btn" type="submit">Register</button>
      </h3>
      <Footer/>
    </form>
  </div>
</template>

<script>


import Menu from "@/components/Header";
import Footer from "@/components/Footer";

export default {
  components: {Footer, Menu},
  data() {
    return {
      user: {
        name: '',
        email: '',
        password: '',
      },
      passwordConfirmation: '',
      submitting: false,
      error: false,
    }
  },

  methods: {
    handleSubmit: function () {
      this.submitting = true
      this.clearStatus()

      if (this.invalidName || this.invalidEmail || this.invalidPassword || this.invalidRepeatPassword) {
        this.error = true
        return
      }

      // check if user exists
      this.userExists(this.user)

    },
    clearStatus: function () {
      this.error = false
    },

    async userExists(user) {
      if (await this.$store.dispatch('user/userExists', user)) {
        this.error = true
        this.submitting = false
      } else {
        // add user
        await this.addUser()
      }
    },
    async addUser() {
      if (await this.$store.dispatch('user/addUser')) {
        //success: new user registered
        await this.$router.push('/message/4')
      } else {
        this.error = true
        this.submitting = false
      }

    },
    cancel() {
      this.$router.push('/MicroPosts')
    },
  },

  computed: {
    invalidName: function () {
      return this.user.name === '' || this.user.name.match(/[0-9]/g) != null;
    },

    invalidPassword: function () {
      return this.user.password === '' || this.user.password.length < 6;
    },

    invalidRepeatPassword: function () {
      return this.submitting && this.user.password !== this.passwordConfirmation;
    },

    invalidEmail: function () {
      const regExpr = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/
      return !this.user.email.match(regExpr);
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