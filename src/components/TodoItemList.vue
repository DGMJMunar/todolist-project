<template>
  <div>
    <todo
      v-for="(todo, index) in todos"
      v-bind:key="index"
      v-bind:todoItem="todo"
    >
    </todo>
    <hr />
    <todo-input v-on:add-Todo="addToTodoArray"></todo-input>
  </div>
</template>

<script>
import Todos from "./Todos.vue";
import TodoItemInput from "./TodoItemInput.vue";
export default {
  components: {
    "todo-input": TodoItemInput,
    todo: Todos,
  },
  data() {
    return {
      todos: [],
    };
  },
  methods: {
    capitalizeLetter(item) {
      return item.charAt(0).toUpperCase() + item.slice(1);
    },
    addToTodoArray(todo) {
      let todoItem = this.capitalizeLetter(todo);
      if (this.todos.includes(todoItem)) {
        alert(`Already added a todo called ${todoItem}`);
        return;
      }
      this.todos.push(todoItem);
      console.log(this.todos);
      this.EventBus.$emit("added-new-todo", todoItem);
      //   alert(todo);
    },
    renderNewTodo(todoArray) {
      console.log(this.todos);
      this.todos = [];
      console.log(this.todos);
      console.log(todoArray);
      this.todos = todoArray;
    },
    sortAsc() {
      this.todos.sort();
      console.log(this.todos);
    },
    sortDesc() {
      this.todos.sort();
      this.todos.reverse();
      console.log(this.todos);
    },
    sortDone() {
      let tempArray = [];
      this.todos.forEach((element, index) => {
        if (element.todoStatus == true) {
          tempArray.push(element);
          this.todos.splice(index, 1);
        }
      });
      Array.prototype.push.apply(this.todos, tempArray);
    },
    sortNotDone() {
      let tempArray = [];
      this.todos.forEach((element, index) => {
        if (element.todoStatus == false) {
          tempArray.push(element);
          this.todos.splice(index, 1);
        }
      });
      Array.prototype.push.apply(this.todos, tempArray);
    },
  },
  mounted() {
    this.EventBus.$on("render-new-todo", this.renderNewTodo);
    this.EventBus.$on("sort-todo-asc", this.sortAsc);
    this.EventBus.$on("sort-todo-desc", this.sortDesc);
    this.EventBus.$on("sort-todo-done", this.sortDone);
    this.EventBus.$on("sort-todo-not-done", this.sortNotDone);
  },
};
</script>

<style scoped>
div {
  display: flexbox;
}
hr {
  height: 1px;
  background-color: #ccc;
  border: none;
}
</style>