<template>
  <header>
    <div class="logo">Todoer</div>
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>

      <button @click="handleLogout" v-if="loggedIn">Log Out</button>
    </nav>
  </header>
  <div class="content">
    <router-view :loggedIn="loggedIn" @login="() => handleLogin(username)"/>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const loggedIn = ref(false)
const user = ref(null)

const handleLogin = (username) => {
  loggedIn.value = true
  user.value = username
}

const handleLogout = () => {
  loggedIn.value = false
  user.value = null

  localStorage.removeItem('current_user')
}
</script>

<style>
body {
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

header {
  display: flex;
  justify-content: space-between;
  padding: 30px;
  background-color: #2c3e50;
  color: white;
}

nav {
  margin-right: 30px;
}

nav button {
  border: 1px solid white;
  color: white;
  background-color: transparent;
  padding: 8px 10px;
  cursor: pointer;
  font-weight: bold;
}

nav a {
  font-weight: bold;
  color: white;
  margin-right: 20px;
  text-decoration: none; 
}

nav a.router-link-exact-active {
  border-bottom: 1px solid white;
  padding-bottom: 5px;
}

.logo {
  font-weight: bold;
  text-transform: uppercase;
  font-size: larger;
}

.content {
  margin: 50px 0;
}
</style>
