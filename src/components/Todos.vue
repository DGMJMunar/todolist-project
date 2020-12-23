<template>
  <div>
    <hr />
    <div id="todoItemParent">
      <b-form-checkbox
        id="checkbox-1"
        name="checkbox-1"
        :value="todoStatus"
        :unchecked-value="!todoStatus"
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
    todoStatus: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      statusName: this.todoStatus,
      dropdownName: "Not Done",
    };
  },
  methods: {
    toggleStatus(status) {
      if (status) {
        this.dropdownName = "Done";
      } else {
        this.dropdownName = "Not Done";
      }
      console.log(this.todoStatus);
    },
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