<template>
  <div>
    <hr />
    <div id="todoItemParent">
      <b-form-checkbox
        id="checkbox-1"
        name="checkbox-1"
        :value="true"
        :checked="checkedStatus"
        :unchecked-value="false"
        v-on:change="changeSelectedStatus"
      >
      </b-form-checkbox>
      <div v-on:click="editItem" id="todoItem">
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
      checkedStatus: false,
      newItem: '',
    };
  },
  methods: {
    editItem(){
      this.newItem = prompt("Change Todo Item", this.todoItem);
      this.EventBus.$emit('edit-todo-item',this.todoItem, this.newItem);
    },
    toggleStatus(status) {
      if (status) {
        this.renameStatus(true);
        this.EventBus.$emit('send-done-todo', this.todoItem)
      } else {
        this.renameStatus(false);
        this.EventBus.$emit('send-not-done-todo', this.todoItem)
      }
      console.log(this.todoStatus);
    },
    renameStatus(bool){
      if(bool){
        this.dropdownName = "Done";
        this.todoStatus = true;
      }
      else{
        this.dropdownName = "Not Done";
        this.todoStatus = false;
      }
    },
    setStatusToNotDone(item){
      if(item == this.todoItem){
        this.renameStatus(false);
      }
    },
    setStatusToDone(item){
      if(item == this.todoItem){
        this.renameStatus(true);
      }
    },
    setSelected(){
      console.log("received setSelected Event");
      this.checkedStatus = true;
    },
    setUnselected(){
      console.log("received setUnselected Event");
      this.checkedStatus = false;
    },
    changeSelectedStatus(){
      this.checkedStatus = !this.checkedStatus
      this.EventBus.$emit('set-todo-selected', this.todoItem);
      console.log(this.checkedStatus)
    }
    // returnTodoStatus(){
    //   if(this.todoStatus){
    //     this.EventBus.$emit('send-done-todo', this.todoItem)
    //   }
    // }
  },
  mounted() {
    this.EventBus.$on('set-status-not-done', this.setStatusToNotDone);
    this.EventBus.$on('set-status-done', this.setStatusToDone);
    this.EventBus.$on('mod-select-all', this.setSelected);
    this.EventBus.$on("mod-unselect-all", this.setUnselected);
  },
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