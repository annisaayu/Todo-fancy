<template>
  <div class="container">
    <div class="register-container">
      <div id="output"></div>
      <h3> Register </h3>
      <div class="form-box">
        <form>
          <input name="email" type="email" placeholder="email" id="email" v-model="email">
          <input type="password" id="pwd" v-model="password" placeholder="password">
          <button type="button" class="btn btn-info btn-block login" @click="register">Register</button>
        </form>
        <div class="tologin">
        <a href='#/login'>Already have account? Login here</a>
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
        this.$router.push('/login')
      }
    }
  }
}
</script>

<style scoped>
html,body{
    position: relative;
    height: 100%;
}

.register-container{
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

.register-container::before,.register-container::after{
    content: "";
    position: absolute;
    width: 100%;height: 100%;
    top: 3.5px;left: 0;
    background: #fff;
    z-index: -1;
    transform: rotateZ(4deg);
    border: 1px solid #ccc;

}

.register-container::after{
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