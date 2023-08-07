<template>
  <form @submit.prevent="handleSubmit">
    <label>Username:</label>
    <input v-model="username" type="text" required>
    <div v-show="error" class="error">{{ error }}</div>

    <div class="submit">
      <button>Log In</button>
    </div>
  </form>
</template>

<script setup>
import { ref } from 'vue'

const username = ref(null)
const error = ref(null)
const emit = defineEmits(['login'])

const handleSubmit = () => {
  const formattedUsername = username.value.replace(' ', '')

  if (formattedUsername.length) {
    if (formattedUsername.length < 3) {
      error.value = 'Username is too short... Get creative!'
    } else {
      ( localStorage.getItem(formattedUsername) ) ? null : localStorage.setItem(formattedUsername, JSON.stringify( { user: formattedUsername, tasks: [] }) )
      localStorage.setItem('current_user', formattedUsername)

      emit('login', formattedUsername)
    }
  } else {
    error.value = 'Please enter your username to continue.'
  }
}
</script>

<style scoped>
form {
  max-width: 420px;
  margin: 30px auto;
  background: #2c3e50;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
}

label {
  color: #aaa;
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 1em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

button {
  background: transparent;
  border: 1px solid white;
  padding: 10px 20px;
  margin-top: 20px;
  color: white;
  cursor: pointer;
  font-weight: bold;
}

.submit {
  text-align: center;
}

input {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}

.error {
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
}
</style>
