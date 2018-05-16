<template>
  <div class="container">
    <h3>Login: </h3>
    <form class= "col-md-4 form-login">
      <div class="form-group">
        <label for="email" class="labelRegis">Email:</label>
        <input type="email" class="form-control" id="email" v-model="email">
      </div>
      <div class="form-group">
        <label for="pwd" class="labelRegis">Password:</label>
        <input type="password" class="form-control" id="pwd" v-model="password">
      </div>
      <button type="button" class="btn btn-default" @click="login">Login</button>
      <!-- <fb:login-button id="fbLogin" scope="public_profile, email" onlogin="checkLoginState();">
            </fb:login-button> -->
      <button v-on:click="loginfb()" class="btn btn-info">Login with Facebook</button>
      <div class="toregister">
        <a href="#/register">Don't have account? Register here</a>
      </div>
    </form>
    <!-- <div class="login-container">
      <div id="output"></div>
      <h3>Login </h3>
      <div class="form-box">
        <form action="" method="">
          <input type="email" name="email"  placeholder="email" id="email" v-model="email">
          <input type="password" id="pwd" v-model="password" placeholder="password">
          <button class="btn btn-info btn-block login" type="submit" @click="login">Login</button>
          <button v-on:click="loginfb()" class="btn btn-primary btn-block login">Login with Facebook</button>
        </form>
        <div class="toregister">
          <a href="#/register">Don't have account? Register here</a>
        </div>
      </div>
    </div> -->
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Login',
  data() {
    return {
      email: '',
      password: ''
    }
  },
  created: function () {
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
        appId: '975855832585758',
        cookie: true,
        xfbml: true,
        version: 'v2.8'
      })
    }
  },
  methods: {
    login() {
      if (this.email && this.password) {
        const objLogin = {
          email: this.email,
          password: this.password
        }
        axios.post('http://localhost:3000/signin', objLogin)
          .then((response) => {
            console.log(response)
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
    loginfb() {
      window.FB.login((response) => {
        console.log('statusChangeCallback')
        console.log(response)
        if (response.status === 'connected') {
          console.log('masuk ke if di login fb')
          localStorage.setItem('fb_access_token', response.authResponse.accessToken)
          this.testAPI()
        } else {
          alert('please login')
        }
      })
    },
    testAPI() {
      console.log('Welcome!  Fetching your information.... ')
      // axios.get('http://localhost:3000/signin', {
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
html,body{
    position: relative;
    height: 100%;
}

.login-container{
    position: relative;
    width: 300px;
    margin: 100px auto;
    padding: 20px 40px 40px;
    text-align: center;
    background: #fff;
    border: 1px solid #ccc;
}

#output{
    position: absolute;
    width: 300px;
    top: -75px;
    left: 0;
    color: #fff;
}

#output.alert-success{
    background: rgb(25, 204, 25);
}

#output.alert-danger{
    background: rgb(228, 105, 105);
}


.login-container::before,.login-container::after{
    content: "";
    position: absolute;
    width: 100%;height: 100%;
    top: 3.5px;left: 0;
    background: #fff;
    z-index: -1;
    transform: rotateZ(4deg);
    border: 1px solid #ccc;

}

.login-container::after{
    top: 5px;
    z-index: -2;
    transform: rotateZ(-2deg);

}

.avatar{
    width: 100px;height: 100px;
    margin: 10px auto 30px;
    border-radius: 100%;
    border: 2px solid #aaa;
    background-size: cover;
}

.form-box input{
    width: 100%;
    padding: 10px;
    text-align: center;
    height:40px;
    border: 1px solid #ccc;;
    background: #fafafa;
    transition:0.2s ease-in-out;

}

.form-box input:focus{
    outline: 0;
    background: #eee;
}

.form-box input[type="email"]{
    border-radius: 5px 5px 0 0;
    text-transform: lowercase;
}

.form-box input[type="password"]{
    border-radius: 0 0 5px 5px;
    border-top: 0;
}

.form-box button.login{
    margin-top:15px;
    padding: 10px 20px;
}

.animated {
  -webkit-animation-duration: 1s;
  animation-duration: 1s;
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
}

@-webkit-keyframes fadeInUp {
  0% {
    opacity: 0;
    -webkit-transform: translateY(20px);
    transform: translateY(20px);
  }

  100% {
    opacity: 1;
    -webkit-transform: translateY(0);
    transform: translateY(0);
  }
}

@keyframes fadeInUp {
  0% {
    opacity: 0;
    -webkit-transform: translateY(20px);
    -ms-transform: translateY(20px);
    transform: translateY(20px);
  }

  100% {
    opacity: 1;
    -webkit-transform: translateY(0);
    -ms-transform: translateY(0);
    transform: translateY(0);
  }
}

.fadeInUp {
  -webkit-animation-name: fadeInUp;
  animation-name: fadeInUp;
}
</style>
