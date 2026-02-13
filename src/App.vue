<template>
  <div class="container todo-app">
    <h1 class="title">Todo List</h1>
    <form class="form">
      <div class="form__group form__group--todo">
        <input class="form-control" type="text" v-model="inputText" placeholder="Новая задача"/>
        <button class="btn btn--add-todo" type="button" @click="addTodo">Добавить</button>
      </div>
    </form>
    <ToDoList
      @remove-todo="removeTodo"
      @complete="setCompleted" :todos="todos"
    />
    <div class="todo-app__footer" v-if="todos.length">
      <p class="todo-app__footer-text">Осталось {{ todosNotCompletedCount }} задания(й)</p>
      <button class="btn btn--clear" @click="clearCompleted()">Удалить завершенные</button>
      <button class="btn btn--clear" @click="clearAll()">Очистить список</button>
    </div>
  </div>
</template>
<script setup>
import { computed, reactive, ref } from 'vue'
import ToDoList from './components/ToDoList.vue'

const inputText = ref('')

const todos = reactive([
  {
    id: 1,
    text: 'Изучить компоненты Vue.js',
    completed: false,
  },
  {
    id: 2,
    text: 'Создать TodoList приложение',
    completed: false,
  },
  {
    id: 3,
    text: 'Похвалить себя за отличную работу',
    completed: false,
  },
])

const addTodo = () => {
  if(!inputText.value.length) {
    return
  }
  const id = new Date();
  const item = { id, text:inputText.value, completed: false }
  todos.push(item)
  inputText.value = ''
}

const removeTodo = (id) => {
  const index = todos.findIndex((item) => item.id === id)
  todos.splice(index, 1)
}

const setCompleted = (id) => {
  const item = todos.find((item) => item.id === id)
  item.completed = true
}
const todosNotCompletedCount = computed(() => todos.filter((item) => !item.completed).length)

const clearAll = () => {
  todos.splice(0, todos.length)
}
const clearCompleted = () => {
  for (let i = todos.length - 1; i >= 0; i--) {
    if (todos[i].completed) {
      todos.splice(i, 1)
    }
  }
}
</script>
<style src="./assets/styles/App.css"></style>
