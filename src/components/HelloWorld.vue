<template>
  <div>
    <h2>Todo list</h2>
    <span>
      <input type="text" v-model="inputFormTitle" v-on:keyup.enter="addTodo">
      <button v-on:click="addTodo">Add todo</button>
    </span>
    <hr>
    <ol>
      <li v-for="todo in todos" v-bind:key="todo.id">
        <input type="checkbox" v-bind:checked="todo.done" v-on:change="toggle(todo)">
        <span v-if="!todo.done">{{ todo.title}}</span>
        <del v-else>{{ todo.title }}</del>
        <button v-on:click="deleteTodo(todo)">delete</button>
      </li>
    </ol>
  </div>
</template>

<script>
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
      inputFormDescription: null
    };
  },
  methods: {
    toggle(item) {
      item.done = !item.done;
    },
    addTodo() {
      this.todos.push({
        id: this.todos.length + 1,
        title: this.inputFormTitle,
        done: false,
      })
      // eslint-disable-next-line
      console.log(this.todos.length);
      this.inputFormTitle = '';
    },
    deleteTodo(item) {
      this.todos = this.todos.filter(function (element) {
        return item.id !== element.id
      })
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
