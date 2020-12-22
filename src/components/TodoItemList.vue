<template>
<div>
    <todo v-for="(todo, index) in todos" 
          v-bind:key="index"
          v-bind:todoItem="todo">
    </todo>
    <hr>
    <todo-input v-on:add-Todo="addToTodoArray"></todo-input>
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
     capitalizeLetter(item){
       return item.charAt(0).toUpperCase() + item.slice(1);
     },
    addToTodoArray(todo){
      let todoItem = this.capitalizeLetter(todo);
      if(this.todos.includes(todoItem)) {
        alert(`Already added a todo called ${todoItem}`)
        return;
      }
      this.todos.push(todoItem);
      console.log(this.todos)
      this.EventBus.$emit('added-new-todo', todoItem);
    //   alert(todo);
    },
    renderNewTodo(todoArray){
      console.log(this.todos)
      this.todos = [];
      console.log(this.todos)
      console.log(todoArray)
      this.todos = todoArray;
    }
  },
  mounted (){
    this.EventBus.$on('render-new-todo', this.renderNewTodo)
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