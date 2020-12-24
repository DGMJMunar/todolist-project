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
      selectedTodos: [],
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
      this.reassignStatus(false)
      this.todos.sort();
      this.reassignStatus(true)
      this.EventBus.$emit("modify-todo", this.todos);
      console.log(this.todos);
    },
    sortDesc() {
      this.reassignStatus(false)
      this.todos.sort();
      this.todos.reverse();
      this.reassignStatus(true)
      this.EventBus.$emit("modify-todo", this.todos);
      console.log(this.todos);
    },
    sortDone() {
      console.log(this.doneTodos)
      let tempArray = [];
      for(let i = 0; i < this.doneTodos.length; i++){
        let index = this.todos.indexOf(this.doneTodos[i])
        let item = this.todos[index]
        tempArray.push(item);
        this.statusEmitter(false, item)
        this.todos.splice(index, 1);
      }
      this.$nextTick(() => {
        for(let j = tempArray.length - 1; j >= 0; j--){
          this.todos.unshift(tempArray[j]);
          this.statusEmitter(true, tempArray[j])
        }
      })
      this.EventBus.$emit("modify-todo", this.todos);
    },
    sortNotDone() {
      console.log(this.doneTodos)
      let tempArray = [];
      for(let i = 0; i < this.doneTodos.length; i++){
        let index = this.todos.indexOf(this.doneTodos[i])
        let item = this.todos[index]
        tempArray.push(item);
        this.statusEmitter(false, item)
        this.todos.splice(index, 1);
      }
      this.$nextTick(() => {
        for(let j = 0; j < tempArray.length; j++){
        this.todos.push(tempArray[j]);
        this.statusEmitter(true, tempArray[j])
      }
      })
      this.EventBus.$emit("modify-todo", this.todos);
      
    },
    setDone(todoItem){
      if(!this.doneTodos.includes(todoItem)){
        this.doneTodos.push(todoItem)
        this.EventBus.$emit('modify-done-todo', this.doneTodos)
      }
    },
    setNotDone(todoItem){
      if(this.doneTodos.includes(todoItem)){
        this.doneTodos.splice(this.doneTodos.indexOf(todoItem), 1);
        this.EventBus.$emit('modify-done-todo', this.doneTodos)
      }
    },
    statusEmitter(bool, item){
      this.$nextTick(() =>{
        if(bool){
          this.EventBus.$emit('set-status-done', item);
        }
        else{
          this.EventBus.$emit('set-status-not-done', item);
        }
      })
    },
    reassignStatus(bool){
      if(bool){
        this.doneTodos.forEach(element => {
          this.statusEmitter(true, element)
        })
      }
      else{
        this.doneTodos.forEach(element => {
          this.statusEmitter(false, element)
        })
      }
    },
    editTodoItem(previousItem,item){
      this.todos[this.todos.indexOf(previousItem)] = item;
      this.renderNewTodo(this.todos)
    },
    setSelected(todoItem){
      if(!this.selectedTodos.includes(todoItem)){
        this.selectedTodos.push(todoItem)
      }
    },
    setUnselected(todoItem){
      if(this.selectedTodos.includes(todoItem)){
        this.selectedTodos.splice(this.selectedTodos.indexOf(todoItem), 1);
      }
    },
    deleteSelected(){
      if(this.selectedTodos.length == 0) return;
      
      console.log("DELETE SELECTED")
      this.selectedTodos.forEach((element, index) => {
        console.log(element);
        this.todos.splice(index, 1);
      })
    },
    deleteAll(){
      this.todos = [];
      this.doneTodos = [];
      this.EventBus.$emit("modify-todo", []);
      this.EventBus.$emit("modify-done-todo", []);
    }
  },
   
  mounted() {
    this.EventBus.$on("render-new-todo", this.renderNewTodo);
    // Sort Events
    this.EventBus.$on("sort-todo-asc", this.sortAsc);
    this.EventBus.$on("sort-todo-desc", this.sortDesc);
    this.EventBus.$on("sort-todo-done", this.sortDone);
    this.EventBus.$on("sort-todo-not-done", this.sortNotDone);
    // Status Change Events
    this.EventBus.$on("send-done-todo", this.setDone)
    this.EventBus.$on("send-not-done-todo", this.setNotDone)
    //Item Edit Events
    this.EventBus.$on("edit-todo-item", this.editTodoItem);
    this.EventBus.$on("set-todo-selected", this.setSelected)
    this.EventBus.$on("set-todo-unSelected", this.setUnselected)
    this.EventBus.$on("mod-delete-selected", this.deleteSelected);
    this.EventBus.$on("mod-delete-all", this.deleteAll);
    
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