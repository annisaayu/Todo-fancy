<template>
<div class="row">
  <div class="col s12 l4 offset-l4">
    <div class="card grey lighten-3">
      <div class="card-content">
        <h3 class="card-title center-align">Register</h3>
        <form>
          <div class="row">
            <div class="input-field col s12">
              <i class="material-icons prefix">email</i>
              <input type="email" id="email" v-model="email" class="validate" required />
              <label for="email">Email</label>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <i class="material-icons prefix">vpn_key</i>
              <input type="password" id="password" v-model="password" class="validate" required />
              <label for="email">Password</label>
            </div>
          </div>
          <div class="row center-align">
            <button class="btn waves-effect waves-light" type="submit" name="action" @click="register">Register
            </button>
          </div>
        </form>
        <div class="toLogin">
          <a href="#/login" class="center-align">Already have account? Login here</a>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Register',
  data () {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    toLogin () {
      this.$router.push('/login')
    },
    register () {
      if (this.email && this.password) {
        const objLogin = {
          email: this.email,
          password: this.password
        }
        axios.post('http://localhost:3000/signup', objLogin)
          .then((response) => {
            console.log(response)
            alert('signup success')
            this.$router.push('/login')
          })
          .catch((err) => {
            alert('signup failed')
            console.log(err)
          })
      } else {
        alert('enter email and password')
      }
    }
  },
  mounted: () => {
    document.body.className = 'login'
  }
}
</script>

<style scoped>
</style>
