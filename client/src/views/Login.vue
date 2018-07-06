<template>
<div class="row">
  <div class="col s12 l4 offset-l4">
    <div class="card grey lighten-3">
      <div class="card-content">
        <h3 class="card-title center-align">Login</h3>
        <form>
          <div class="row">
            <div class="input-field col s12">
              <i class="material-icons prefix">email</i>
              <input type="email" id="email" class="validate" v-model="email" required />
              <label for="email">Email</label>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <i class="material-icons prefix">vpn_key</i>
              <input type="password" id="password" class="validate" v-model="password" required />
              <label for="email">Password</label>
            </div>
          </div>
          <div class="row center-align">
            <button class="btn waves-effect waves-light" type="submit" @click="login()" name="action">Login
            </button>
          </div>
          <button @click="loginfb()" class="btn btn-info">Login with Facebook</button>
        </form>
        <a href="#/register" class="center-align">Don't have account? Register here</a>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Login',
  data () {
    return {
      email: '',
      password: ''
    }
  },
  created: function () {
    if (localStorage.hasOwnProperty('token')) {
      this.$router.push('/todo')
    } else {
      // FB SDK
      (function (d, s, id) {
        var js
        var fjs = d.getElementsByTagName(s)[0]
        if (d.getElementById(id)) return
        js = d.createElement(s)
        js.id = id
        // js.src = "//connect.facebook.net/en_US/sdk.js#xfbml=1&version=v2.10&appId=119308148780939"
        js.src = '//connect.facebook.net/en_US/sdk.js'
        fjs.parentNode.insertBefore(js, fjs)
      }(document, 'script', 'facebook-jssdk'))
      window.fbAsyncInit = function () {
        window.FB.init({
          appId: '1369761109791179',
          cookie: true,
          xfbml: true,
          version: 'v2.8'
        })
      }
    }
  },
  methods: {
    login () {
      if (this.email && this.password) {
        let objLogin = {
          email: this.email,
          password: this.password
        }
        console.log(objLogin)
        axios.post('http://localhost:3000/signin', objLogin)
          .then((response) => {
            localStorage.setItem('token', response.data.token)
            this.$router.push('/todo')
          })
          .catch((err) => {
            alert('login failed')
            console.log(err)
          })
      } else {
        alert('enter email and password')
      }
    },
    loginfb () {
      window.FB.login((response) => {
        console.log('statusChangeCallback')
        console.log(response)
        if (response.status === 'connected') {
          localStorage.setItem('fb_access_token', response.authResponse.accessToken)
          this.testAPI()
        } else {
          alert('please login')
        }
      })
    },
    testAPI () {
      console.log('Welcome!  Fetching your information.... ')
      axios.get('http://localhost:3000/loginFb', {
        headers: { fb_access_token: localStorage.getItem('fb_access_token') }
      })
        .then((response) => {
          console.log('data dari server', response.data)
          localStorage.setItem('token', response.data.token)
          this.$router.push('/todo')
        })
        .catch((err) => {
          console.log(err)
        })
    }
  },
  mounted: () => {
    document.body.className = 'login'
  }
}
</script>

<style scoped>
</style>
