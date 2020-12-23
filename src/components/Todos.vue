<template>
  <div>
    <hr />
    <div id="todoItemParent">
      <b-form-checkbox
        id="checkbox-1"
        name="checkbox-1"
        :value="true"
        :unchecked-value="false"
      >
      </b-form-checkbox>
      <div id="todoItem">
        <p>{{ todoItem }}</p>
      </div>
    </div>
    <div id="todosItemContainer">
      <div>
        <b-dropdown
          id="dropdown-left"
          :text="dropdownName"
          size="lg"
          class="m-2"
        >
          <b-dropdown-item-button v-on:click="toggleStatus(true)"
            >Done</b-dropdown-item-button
          >
          <b-dropdown-item-button v-on:click="toggleStatus(false)"
            >Not Done</b-dropdown-item-button
          >
          <!-- <b-dropdown-divider></b-dropdown-divider> -->
        </b-dropdown>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    todoItem: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      dropdownName: "Not Done",
      todoStatus: false,
    };
  },
  methods: {
    toggleStatus(status) {
      if (status) {
        this.dropdownName = "Done";
        this.todoStatus = true;
        this.EventBus.$emit('send-done-todo', this.todoItem)
      } else {
        this.dropdownName = "Not Done";
        this.todoStatus = true;
        this.EventBus.$emit('send-not-done-todo', this.todoItem)
      }
      console.log(this.todoStatus);
    },
    // returnTodoStatus(){
    //   if(this.todoStatus){
    //     this.EventBus.$emit('send-done-todo', this.todoItem)
    //   }
    // }
  },
  mounted() {
    // this.EventBus.$on("get-done-todo", this.returnTodoStatus);
  },
  // watch:{
  //   "todoStatus" : function(){
  //     if(this.todoStatus) {
  //       this.EventBus.$emit('send-done-todo', this.todoItem)
  //     }
  //     else {
  //       this.EventBus.$emit('send-not-done-todo', this.todoItem)
  //     }
  //   }
  // }
};
</script>

<style>
#todoItemParent {
  display: inline-flex;
  justify-content: flex-start;
  width: 87.5%;
}
#todosItemContainer {
  display: inline-flex;
  width: 12.5%;
  justify-content: center;
}
#todoItem {
  margin-left: 15px;
  display: flex;
  justify-content: space-between;
}
hr {
  height: 1px;
  background-color: #ccc;
  border: none;
}
p {
  color: #a0a0a0;
  font-size: 25px;
  text-transform: capitalize;
}
</style>