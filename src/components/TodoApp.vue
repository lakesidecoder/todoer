<template>
    <div class="greeting">
        <h1>Hello, {{ username }}!<br>
        What would you like to do, today?</h1>

        <form @submit.prevent="addTask" class="new-todo">
            <input v-model="task" type="text"/>
            <button>Add Task</button>
        </form>
    </div>

    <div class="tasks">
        <ul v-for="task in tasks">
            <li :class="{ 'completed': task.status === 'completed' }"><input @click="toggleTaskStatus(task)" type="checkbox"/>{{ task.title }} <button @click="editTask(task)">Edit</button><button @click="deleteTask(task)">Delete</button></li>
        </ul>
    </div>
</template>

<script setup>
import { defineProps, ref } from 'vue'

const props = defineProps({ username: String })

const username = ( props.username ) ? props.username : localStorage.getItem('current_user')

let task = null
const userData = JSON.parse(localStorage.getItem(username))
const tasks = ref(userData.tasks)

const updateStorage = () => {
    localStorage.setItem(username, JSON.stringify({user: username, tasks: tasks.value}))
}

const addTask = () => {
    tasks.value.push({title: task, status: 'pending'})

    updateStorage()

    task = null
}

const deleteTask = (task) => {
    const sort = tasks.value.filter((t) => t.title !== task.title)

    tasks.value = sort

    updateStorage()
}

const editTask = (task) => {
    task.title = prompt('What would you like to rename this task to?')

    updateStorage()
}

const toggleTaskStatus = (task) => {
    task.status = ( task.status === 'pending' ) ? 'completed' : 'pending'

    updateStorage()
}
</script>

<style scoped>
.new-todo input {
    padding: 10px;
    transition: width .5s ease-in
}

.new-todo input:focus {
    width: 400px;
    transition: width .5s ease-out
}

.new-todo button {
    padding: 10px;
    background-color: transparent;
    color: black;
    border: 1px solid black;
    border-left: none;
    cursor: pointer;
}

input[type='checkbox'] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
    padding: 15px 6px;
}

.tasks {
    width: 400px;
    margin: 35px auto;
}

.tasks ul li {
    list-style-type: none;
    font-size: 30px;
}

.completed {
    text-decoration: line-through;
}
</style>