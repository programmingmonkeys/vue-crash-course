<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    // Lint failure:
    // deleteTodo(id) {
    //   axios.delete(`${process.env.VUE_APP_URL}/${id}`)
    //     .then(res => this.todos = this.todos.filter(todo => todo.id !== id))
    //     .catch(err => console.log(err));
    // },
    deleteTodo(id) { 
      axios.delete(`${process.env.VUE_APP_URL}/${id}`)
        .then(this.todos = this.todos.filter(todo => todo.id !== id)) 
        .catch(err => console.log(err))},
    addTodo(newTodo) {
      const { title, completed } = newTodo;

      axios.post(process.env.VUE_APP_URL, {
        title,
        completed
      })
        .then(res => this.todos = [...this.todos, res.data])
        .catch(err => console.log(err));
    }
  },
  created() {
    axios.get(`${process.env.VUE_APP_URL}?_limit=5`)
      .then(res => this.todos = res.data)
      .catch(err => console.log(err));
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }

  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }

  .btn:hover {
    background: #666;
  }
</style>
