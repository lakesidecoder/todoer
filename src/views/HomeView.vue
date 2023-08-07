<template>
  <Login v-if="!loggedIn" @login="(user) => appLogin(user)"/>
  <TodoApp :username="username" v-if="loggedIn"/>
</template>

<script>
import Login from '@/components/Login.vue'
import TodoApp from '@/components/TodoApp.vue'

export default {
  name: 'HomeView',
  props: ['loggedIn'],
  data() {
    return {
      username: null
    }
  },
  components: {
    Login, TodoApp
  },
  methods: {
    appLogin(user) {
      this.username = user
      this.$emit('login', user)
    }
  }
}
</script>
