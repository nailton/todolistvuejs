<template>
  <div id="app">
    <Header />
    <AddTodo @add-todo="addTodo"/>
    <Todos v-bind:todos="todos" @del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';
import axios from 'axios';
export default {
  name: 'App',
  components: {
    Todos,
    AddTodo
  },
  data(){
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id){
      axios.delete(`https://jsonplaceholder.cypress.io/todos/${id}`)
       // .then(res => this.todos = this.todos.filter(todo => todo.id !== id))
       .then(this.todos = this.todos.filter(todo => todo.id !== id))
       .catch(err => console.log(err));
    },
    addTodo(newTodo){
      const {title, completed} = newTodo;

      axios.post('https://jsonplaceholder.cypress.io/todos', {
        title,
        completed
      })
      .then(res => this.todos = [...this.todos, res.data])
      .catch(err => console.log(err));

    }
  },
  created(){
axios.get('https://jsonplaceholder.cypress.io/todos?_limit=5')
.then(res => this.todos = res.data)
.catch(err => console.log(err))
  }
}
</script>


