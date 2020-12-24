<template>
  <div id="app">
    <div id="mainContainer">
      <div id="leftSide">
        <h1 class="text-left title">TO-DO LIST</h1>
        <category-list></category-list>
      </div>

      <div id="rightSide">
        <div id="usernameContainer">
          <input
            type="text"
            :placeholder="inputString"
            id="usernameInput"
            disabled="disabled"
          />
        </div>
        <hr>
        <div id="modifierContainer" v-if="hasCategories">
          <div id="modifierChild-1" >
            <!-- <div> -->
              <b-dropdown
                id="dropdown-right"
                text="Filter"
                size="lg"
                class="m-2"
              >
                <b-dropdown-item-button v-on:click="sortAsc"
                  >Sort Ascending</b-dropdown-item-button
                >
                <b-dropdown-item-button v-on:click="sortDesc"
                  >Sort Descending</b-dropdown-item-button
                >
                <b-dropdown-item-button v-on:click="sortDone"
                  >Sort by Done</b-dropdown-item-button
                >
                <b-dropdown-item-button v-on:click="sortNotDone"
                  >Sort by Not Done</b-dropdown-item-button
                >
                <!-- <b-dropdown-divider></b-dropdown-divider> -->
              </b-dropdown>

              <b-button class="modifierButton" v-on:click="selectAll">Select All</b-button>
              <b-button class="modifierButton" v-on:click="unselectAll">Unselect All</b-button>
              <!-- <b-button class="modifierButton" v-on:click="editText">Edit</b-button> -->
              <b-button class="modifierButton" v-on:click="deleteSelected">Delete Selected</b-button>
              <b-button class="modifierButton" v-on:click="deleteAll">Delete All</b-button>
            <!-- </div> -->
          </div>
          <div id="todoStatus">
            <h1>STATUS</h1>
          </div>
          
        </div>
        <div v-if="hasCategories" >
          <hr>
        <h4 >Click todo items to edit</h4>
        </div>
        
        <div id="toDoListContainer">
          <todo-item-list v-if="hasCategories"> </todo-item-list>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoItemList from "./components/TodoItemList.vue";
import CategoryList from "./components/CategoryList.vue";
export default {
  name: "App",
  components: {
    "todo-item-list": TodoItemList,
    "category-list": CategoryList,
  },
  data() {
    return {
      hasCategories: false,
      inputString: "USERNAME",
    };
  },
  methods: {
    sortAsc() {
      this.EventBus.$emit("sort-todo-asc");
    },
    sortDesc() {
      this.EventBus.$emit("sort-todo-desc");
    },
    sortDone() {
      this.EventBus.$emit("sort-todo-done");
    },
    sortNotDone() {
      this.EventBus.$emit("sort-todo-not-done");
    },
    selectAll(){
      this.EventBus.$emit("mod-select-all");
    },
    unselectAll(){
      this.EventBus.$emit("mod-unselect-all");
    },
    editText(){
      this.EventBus.$emit("mod-edit-text");
    },
    deleteSelected(){
      this.EventBus.$emit("mod-delete-selected");
    },
    deleteAll(){
      this.EventBus.$emit("mod-delete-all");
    },
    checkCategories(status) {
      if (status) {
        this.hasCategories = true;
      } else {
        this.hasCategories = false;
      }
    },
  },
  mounted() {
    this.hasCategories = false;
    this.EventBus.$on("check-categories-array", this.checkCategories);
    this.inputString = prompt("Welcome, Enter your name!", "USERNAME");
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #676767;
  background-color: #242424;

  min-height: 100vh;
  height: 100%;
  max-height: auto;
}
#mainContainer {
  display: flex;
  min-height: 100vh;
  height: 100%;
  max-height: auto;
  padding: 25px;
}
#leftSide {
  background-color: #373737;
  width: 15%;
}
.title {
  padding: 20px 30px;
}

#rightSide {
  /* background-color: yellow; */
  width: 85%;
  padding: 20px 30px;
}
#usernameContainer {
  display: flex;
  justify-content: flex-end;
  width: 100%;
}
#usernameInput {
  background-color: none;
  /* background-image: url('searchicon.png'); */
  /* background-position: 10px 10px; */
  /* background-repeat: no-repeat; */
  text-align: end;
  background: transparent;
  border-style: none;
  font-size: 25px;
  color: #727272;
}
::placeholder {
  display: flex;
  justify-self: flex-end;
}
h4{
  font-weight: bold;
  text-align: center;
  text-transform: uppercase;
}
#categoryContainer {
  padding-top: 175px;
  margin-left: -30px;
}

ul {
  list-style: none;
}
li {
  font-size: 24px;
  text-transform: uppercase;
}
#modifierContainer {
  margin-top: 120px;
  display: flex;
  justify-content: space-between;
}
#modifierChild-1{
  display: flex;
  justify-content: space-between;
  width: 40%;
  height: 60px;
}
.modifierButton{
  width: 100px
}
#todoStatus {
  background-color: #404040;
  width: 12.5%;
  display: block;
  text-align: center;
  justify-content: center;
  padding-top: 7px;
}
#todoStatus h1 {
  font-weight: bold;
  font-size: 40px;
}
</style>
