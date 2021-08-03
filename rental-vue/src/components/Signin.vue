<template>
<div>
  <h3>Signin test</h3>
  <form @submit.prevent="signin">
    <div v-if="error">{{ error }}</div>
    <div>
      <label>E-mail adress</label>
      <input type="email" v-model="email" id='email' placeholder="Enter email">
    </div>
        <div>
      <label>Password</label>
      <input type="password" v-model="password" id='password' placeholder="Enter password">
    </div>
    <button type="submit">Submit </button>
    <div>
      <router-link to="/signup">Sign up</router-link>
    </div>
  </form>
</div>
</template>

<script>
export default {
  name: 'Signin',
  data () {
    return {
      email: '',
      password: '',
      error: '',
      response: ''
    }
  },
  created () {
    this.checkSignedIn()
  },
  updated () {
    this.checkSignedIn()
  },
  methods: {
    signin () {
      this.$http.plain.post('/signin', { email: this.email, password: this.password })
        .then(response => this.signinSuccesful(response))
        .catch(error => this.signinFailed(error))
    },
    signinSuccesful (response) {
      if (!response.data.csrf) {
        this.signinFailed(response)
        return
      }
      localStorage.csrf = response.data.csrf
      localStorage.signedIn = true
      this.error = ''
      this.$router.replace('/properties')
    },
    signinFailed (error) {
      this.error = (error.response && error.response.data && error.response.data.error) || ''
      delete localStorage.csrf
      delete localStorage.signedIn
    },
    checkSignedIn () {
      if (localStorage.signedIn) {
        this.$router.replace('/properties')
      }
    }
  }
}
</script>

<style>

</style>
