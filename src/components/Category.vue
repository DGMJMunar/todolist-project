<template>
  <div
    id="categoryItemText"
    class="border border-light"
    :class="this.divStyle"
    v-on="eventListeners"
  >
    {{ categoryItem }}
  </div>
</template>

<script>
// import Todos from "./Todos.vue";
// import {EventBus} from "../main.js";
export default {
  props: {
    categoryItem: {
      type: String,
      required: true,
    },
    activeCategory: {
      type: String,
      required: true,
      default: "notThis",
    },
  },
  data() {
    const vm = this;

    return {
      divStyle: "defaultValue",
      eventListeners: {
        click: vm.setActiveTodo,
        "active-event": vm.setActiveTodo,
      },
      todos: [],
      doneTodos: []
    };
  },
  methods: {
    setActiveTodoEvent(item) {
      if (item == this.categoryItem) {
        console.log("ITEM : " + item);
        // this.EventBus.$emit("render-new-todo", this.categoryItem, this.todos);
        this.EventBus.$emit("set-active-category", this.categoryItem);
        if (this.divStyle != "isSelected")
          this.$nextTick(this.updateDivStyle());
      }
    },

    setActiveTodo() {
      console.log("set active todo event");
      // this.EventBus.$emit("render-new-todo", this.categoryItem, this.todos);
      this.EventBus.$emit("set-active-category", this.categoryItem);
      this.EventBus.$emit("reset-category-div");
      this.$nextTick(this.updateDivStyle());
    },

    resetDivToDefault() {
      this.divStyle = "defaultValue";
    },

    updateDivStyle() {
      // console.log("UPDATE DIV STYLE");

      if (this.divStyle === "defaultValue") {
        this.divStyle = "isSelected";
      } else {
        this.divStyle = "defaultValue";
      }
    },

    addTodoItem(todo) {
      console.log(
        `This todo = ${this.activeCategory} while This item = ${this.categoryItem}`
      );
      if (this.activeCategory == this.categoryItem) {
        console.log(this.todos + todo);
      }
    },

    passTodoData(item) {
      if (item == this.categoryItem) {
        this.EventBus.$emit("render-new-todo", this.todos);
      }
    },
    modifyTodo(newArr){
      this.todos = [];
      this.todos = newArr;
    },
    modifyDoneTodos(newArr){
      this.doneTodos = [];
      this.doneTodos = newArr;
    }
    // sortAsc() {
    //   this.todos.sort();
    // },
    // sortDesc() {
    //   this.todos.sort();
    //   this.todos.reverse();
    // },
    // sortDone() {
    //   let tempArray = [];
    //   this.todos.forEach((element, index) => {
    //     if (element.todoStatus == true) {
    //       tempArray.push(element);
    //       this.todos.splice(index, 1);
    //     }
    //   });
    //   Array.prototype.push.apply(this.todos, tempArray);
    // },
    // sortNotDone() {
    //   let tempArray = [];
    //   this.todos.forEach((element, index) => {
    //     if (element.todoStatus == false) {
    //       tempArray.push(element);
    //       this.todos.splice(index, 1);
    //     }
    //   });
    //   Array.prototype.push.apply(this.todos, tempArray);
    // },
  },

  mounted() {
    this.EventBus.$on("reset-category-div", this.resetDivToDefault);
    this.EventBus.$on("active-event", this.setActiveTodoEvent);
    this.EventBus.$on("added-new-todo", this.addTodoItem);
    this.EventBus.$on("pass-todo-data", this.passTodoData);
    this.EventBus.$on("modify-todo", this.modifyTodo);
    this.EventBus.$on('modify-done-todo', this.modifyDoneTodos)
  },
};
</script>

<style scoped>
#categoryItemText {
  /* all: initial; */
  font-weight: 900;
  font-size: 30px;
  text-transform: uppercase;
  text-align: center;
  /* background-color:red; */
}
.defaultValue {
  color: white;
}
.isSelected {
  background-color: #242424;
  color: #646464;
}
</style>