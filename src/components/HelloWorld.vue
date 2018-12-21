<template>
  <div>
    <h2>Todo list</h2>
    <span>
      <label>Title</label>
      <input type="text" v-model="inputFormTitle" v-on:keyup.enter="createTodo">
      <label>Description:</label>
      <input type="text" v-model="inputFormDescription" v-on:keyup.enter="createTodo">
      <label>Create by</label>
      <input type="text" v-model="inputFormCreatedBy" v-on:keyup.enter="createTodo">
      <button v-on:click="createTodo">Add todo</button>
    </span>
    <hr>
    <ol>
      <li v-for="todo in todos" v-bind:key="todo.id">
        <input
          type="checkbox"
          v-bind:checked="todo.done"
          v-on:change="updateTodoToApi(todo)"
        >
        <span
          v-if="!todo.done"
        >title: {{ todo.title}}, description: {{ todo.description }}, created by {{ todo.createdBy }}</span>
        <del
          v-else
        >title: {{ todo.title}}, description: {{ todo.description }}, created by {{ todo.createdBy }}</del>
        <button v-on:click="deleteTodoToApi(todo)">delete</button>
      </li>
    </ol>
    <button v-on:click="getTodoFromApi">Get Todo List</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  data() {
    return {
      todos: [
        {
          id: 0,
          title: "Play video games",
          description: "Drop an exam and go play video games",
          done: false,
          createdBy: "5809610347"
        },
        {
          id: 1,
          title: "Learn vue js",
          description: "Learn vue js is so easy.",
          done: true,
          createdBy: "5809610347"
        }
      ],
      inputFormTitle: null,
      inputFormDescription: null,
      inputFormCreatedBy: null
    };
  },
  methods: {
    toggleTodoAndSendToApi(item) {
      item.done = !item.done;
    },
    createTodo() {
      const body = {
        title: this.inputFormTitle,
        description: this.inputFormDescription,
        createdBy: this.inputFormCreatedBy,
        done: false
      };
      axios
        .post("http://localhost:3000/todo", body)
        .then(res => res.data)
        .then(res => {
          // eslint-disable-next-line
          console.log(res);
          this.getTodoFromApi();
        });
      this.inputFormTitle = "";
      this.inputFormDescription = "";
      this.inputFormCreatedBy = "";
    },
    getTodoFromApi() {
      axios
        .get("http://localhost:3000/todo")
        .then(res => res.data)
        .then(res => {
          // eslint-disable-next-line
          console.log(res);
          this.todos = res;
        });
    },
    updateTodoToApi(todo) {
      todo.done = !todo.done;
      axios
        .put(`http://localhost:3000/todo/${todo.id}`, todo)
        .then(res => res.data)
        .then(res => {
          // eslint-disable-next-line
          console.log(res);
          this.getTodoFromApi()
        });
    },
    deleteTodoToApi(item) {
      axios
        .delete(`http://localhost:3000/todo/${item.id}`)
        .then(res => res.data)
        .then(res => {
          // eslint-disable-next-line
          console.log(res);
          this.getTodoFromApi();
        });
    }
  },
  mounted() {
    this.getTodoFromApi();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
