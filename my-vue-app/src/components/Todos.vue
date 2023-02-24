<template>
  <AddTodo @add-todo="addTodo" />
  <TodoItem
    v-for="todo in todos"
    :key="todo.id"
    :todoProps="todo"
    @item-completed="markCompleted"
    @delete-item="deleteTodo"
  />
</template>

<script>
import { ref } from 'vue'
import axios from 'axios'
import AddTodo from './AddTodo.vue'
import TodoItem from './TodoItem.vue'

export default {
  name: 'Todos',
  components: {
    TodoItem,
    AddTodo
  },
  setup() {
    const todos = ref([])
    const getAllTodo = async () => {
      try {
        const res = await axios.get(
          'https://jsonplaceholder.typicode.com/todos?_limit=5'
        )
        console.log(res.data)
        todos.value = res.data
      } catch (err) {
        console.log(err)
      }
    }
    getAllTodo()
    const markCompleted = id => {
      todos.value = todos.value.map(todo => {
        if (todo.id === id) {
          todo.completed = !todo.completed
        }
        return todo
      })
      return id
    }
    const deleteTodo = async id => {
      try {
        await axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        todos.value = todos.value.filter( todo => todo.id !== id)
      } catch (error) {
        console.log(error)
      }
    }
    const addTodo = async newTodo => {
      try {
       const res = await axios.post("https://jsonplaceholder.typicode.com/todos", newTodo)
       todos.value.push(res.data);
      } catch (error) {
        console.log(error)
      }
    }
    return {
      todos,
      markCompleted,
      deleteTodo,
      addTodo,
      getAllTodo
    }
  }
}
</script>

<style></style>
