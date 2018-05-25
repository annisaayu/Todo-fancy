<template>
  <nav class="navbar navbar-inverse">
      <div class="container-fluid">
          <div class="navbar-header">
              <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#myNavbar">
                  <span class="icon-bar"></span>
                  <span class="icon-bar"></span>
                  <span class="icon-bar"></span>
              </button>
              <a class="navbar-brand" href="#">Todo</a>
          </div>
          <div class="collapse navbar-collapse" id="myNavbar">
            <ul class="nav navbar-nav">
              <li>
                  <a v-on:click="logout">Logout</a>
              </li>
            </ul>
          </div>
      </div>
  </nav>
</template>

<script>
export default {
  name: 'Navbar'
}
</script>