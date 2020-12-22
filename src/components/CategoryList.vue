<template>
  <div id="categoryContainer">
    <ul id="categories">
      <category-item v-for="(category, index) in categories" 
          v-bind:key="index"
          v-bind:categoryItem="category">
        </category-item>
    </ul>
    <category-input v-on:addCategory="addToCategoryArray"></category-input>
  </div>
</template>

<script>
import Category from "./Category.vue";
import CategoryInput from "./CategoryInput.vue";
export default {
  components: {
    "category-item": Category,
    "category-input": CategoryInput,
  },
  data() {
    return {
      categories: [],
    };
  },
  methods:{
    addToCategoryArray(category){
      console.log("HEYEYEYEYEY")
      this.categories.push(category);
      console.log(this.categories)
    }
  }
};
</script>

<style scoped>
#categoryContainer{
  margin-left: -40px;
}
input[type=text] {
  background-color: none;
  /* background-image: url('searchicon.png'); */
  /* background-position: 10px 10px; */
  /* background-repeat: no-repeat; */
  background: transparent;
  border-style: none;
  font-weight: 900;
  font-size: 21px;
}
::placeholder{
  color:#727272;
  padding-left: 25px;
}

</style>