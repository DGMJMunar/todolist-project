<template>

  <div id="app">
    <div id="mainContainer">
      <div id="leftSide">
        <h1 class="text-left title">TO-DO LIST</h1>
        <category-list></category-list>
        
      </div>

      <div id="rightSide">
        <div id="usernameContainer">
          <input type="text" placeholder="USERNAME" id="usernameInput"/>
        </div>
        <div id='toDoListContainer'>
          <todo-item-list> </todo-item-list>
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
    "todo-item-list" : TodoItemList,
    "category-list" : CategoryList
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
  
  height: 100vh;
}
#mainContainer {
  display: flex;
  height: 100%;
  
}
#leftSide {
  background-color: #373737;
  width: 15%;
}
.title{
  padding: 20px 30px;
}

#rightSide {
  /* background-color: yellow; */
  width: 85%;
  padding: 20px 30px;
}
#usernameContainer{
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
::placeholder{
  display: flex;
  justify-self: flex-end;
}

#categoryContainer{
  padding-top: 175px;
  margin-left: -30px;
}

ul{
  list-style: none;
}
li{
  font-size:24px;
  text-transform: uppercase;
}
#toDoListContainer{
margin-top: 185px;
}

</style>
