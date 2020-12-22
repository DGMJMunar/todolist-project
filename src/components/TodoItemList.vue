<template>
<div>
    <todo v-for="(todo, index) in todos" 
          v-bind:key="index"
          v-bind:todoItem="todo">
    </todo>
    <hr>
    <todo-input v-on:addTodo="addToTodoArray"></todo-input>
</div>
</template>

<script>
import Todos from "./Todos.vue";
import TodoItemInput from "./TodoItemInput.vue";
export default {
  components: {
    "todo-input": TodoItemInput,
    "todo": Todos,
  },
  data(){
    return {
      todos: []
    }
  },
   methods:{
    addToTodoArray(todo){
      this.todos.push(todo);
    //   alert(todo);
    }
  }
};
</script>

<style scoped>
div{
    display: flexbox;
    
}
hr{
    border: 1px solid grey;
}
</style>