<template>
  <div id="app" style="color:black">
    <b-navbar toggleable="md"  variant="dark" type="dark">
      <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>
      <img src="./assets/AgreeOrDisagreeIcon.png" class="img-circle" style="padding: 5px">
      <b-collapse is-nav id="nav_collapse">
        <b-navbar-nav class="ml-auto">
          <b-nav-item to="/#"><i class="fa fa-home" style="padding: 5px"> Home</i></b-nav-item>
          <b-nav-item to="/quiz"><i class="fa fa-question" style="padding: 5px"> The Survey</i></b-nav-item>
        <b-nav-item to="/addStatement"><i class="fa fa-plus" style="padding: 5px"> Add a Statement</i></b-nav-item>
        </b-navbar-nav>
        <b-navbar-nav class="ml-auto">
          <img src="./assets/AgreeOrDisagreeIcon.png" class="img-circle" style="padding: 5px">
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
