<template>
  <div class="max-w-sm m-auto my-8">
    <div class="border p-10 border-grey-light shadow rounded">
      <h3 class="text-2xl mb-6 text-grey-darkest">Sign In</h3>
      <form @submit.prevent="signin">
        <div class="text-red" v-if="error">{{ error }}</div>

        <div class="mb-6">
          <label for="email" class="label">E-mail Address</label>
          <input type="email" v-model="email" class="input" id="email" placeholder="andy@web-crunch.com">
        </div>
        <div class="mb-6">
          <label for="password" class="label">Password</label>
          <input type="password" v-model="password" class="input" id="password" placeholder="Password">
        </div>
        <button type="submit">
          Sign In
        </button>
        <div class="my-4">
          <router-link to="/signup" class="link-grey">Sign up</router-link>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Signin',
  data () {
    return {
      email: '',
      password: '',
      error: '',
      responseData: '',
      counter: 0
    }
  },
  created () {
    this.counter += 1
    this.$store.state.localStorage.count = this.counter
    console.log('count', this.$store.state.localStorage.count)
  },
  updated () {
    this.counter += 1
    console.log('status', this.$store.state.localStorage.status.csrf)
    console.log('signed in', this.$store.state.localStorage.status.signedIn)
  },
  methods: {
    async signin () {
      await axios.post(`${this.$axios.defaults.baseURL}/signin`, {
        email: this.email,
        password: this.password
      })
        .then(response => this.signinSuccessful(response))
        .catch(error => this.signinFailed(error))
    },
    signinSuccessful (response) {
      if (!response.data.csrf) {
        this.signinFailed(response)
        return
      }
      this.$store.state.localStorage.status.csrf = response.data.csrf
      this.$store.state.localStorage.status.signedIn = true
      this.error = ''
    },
    signinFailed (error) {
      this.error = (error.response && error.response.data && error.response.data.error) || ''
      delete this.$store.state.localStorage.status.csrf
      delete this.$store.state.localStorage.status.signedIn
    },
    checkSignedIn () {
      if (typeof window !== 'undefined') {
        if (this.$store.state.localStorage.status.signedIn) {
          alert('signed in!')
        }
      }
    }
  }
}
</script>
