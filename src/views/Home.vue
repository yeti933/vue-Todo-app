<template>
  <div class="home">
    <Header />
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "../components/layout/Header"
import Todos from "../components/Todos"
import AddTodo from "../components/AddTodo";

export default {
  name: "home",
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      console.log(id)
      const url =`https://jsonplaceholder.typicode.com/todos/${id}`;
      const postParam = {
        method: 'DELETE'
      };

      fetch(url, postParam)
        .then( res => this.todos = this.todos.filter( todo => todo.id !== id))
        .catch( err => console.log(err));
    },
    addTodo(Todo) {
      const { title, completed} = Todo;
      const url =`https://jsonplaceholder.typicode.com/todos`;
      const postParam = {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify({title, completed})
      };

      fetch(url, postParam)
        .then( res => res.json())
        .then( res => this.todos = [...this.todos, res])
        .catch( err => console.log(err));
    }
  },
  created() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=10 ')
      .then( res => res.json())
      .then( resJson => this.todos = resJson);
  }
};
</script>
