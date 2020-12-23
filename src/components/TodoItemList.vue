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
      doneTodos: [],
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
      this.EventBus.$on("modify-todo", this.todos);
      console.log(this.todos);
    },
    sortDesc() {
      this.todos.sort();
      this.todos.reverse();
      this.EventBus.$on("modify-todo", this.todos);
      console.log(this.todos);
    },
    sortDone() {
      console.log(this.doneTodos)
      // this.EventBus.$emit('get-done-todo');
      let tempArray = [];
      // this.doneTodos.forEach((element, index) => {
      //     let item = this.todos[this.todos.indexOf(element)]
      //     console.log(item)
      //     tempArray.push(item);
      //     this.todos.splice(index, 1);
        
      // });
      for(let i = 0; i < this.doneTodos.length; i++){
        let index = this.todos.indexOf(this.doneTodos[i])
        let item = this.todos[index]
        tempArray.push(item);
        this.todos.splice(index, 1);
      }
      console.log(`temp array ${tempArray}`)
      console.log(`this.todos array ${this.todos}`)
      // Array.prototype.push.apply(this.todos, tempArray);
      // console.log(`this.todos array ${this.todos}`)
      // this.EventBus.$on("modify-todo", this.todos);
    },
    setDone(todoItem){
      if(!this.doneTodos.includes(todoItem)){
        this.doneTodos.push(todoItem)
        console.log(this.doneTodos)
      }
      
    },
    setNotDone(todoItem){
      this.doneTodos.splice(this.doneTodos.indexOf(todoItem), 1);
      console.log(this.doneTodos)
    },
    sortNotDone() {
      // let tempArray = [];
      this.todos.forEach((element, index) => {
        if (element.todoStatus == false) {
          this.doneTodos.push(element);
          this.todos.splice(index, 1);
        }
      });
      Array.prototype.push.apply(this.todos, this.doneTodos);
      this.EventBus.$on("modify-todo", this.todos);
    },
  },
  mounted() {
    this.EventBus.$on("render-new-todo", this.renderNewTodo);
    this.EventBus.$on("sort-todo-asc", this.sortAsc);
    this.EventBus.$on("sort-todo-desc", this.sortDesc);
    this.EventBus.$on("sort-todo-done", this.sortDone);
    this.EventBus.$on("sort-todo-not-done", this.sortNotDone);
    this.EventBus.$on("send-done-todo", this.setDone)
    this.EventBus.$on("send-not-done-todo", this.setNotDone)
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