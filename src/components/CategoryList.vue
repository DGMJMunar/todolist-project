<template>
  <div id="categoryContainer">
    <ul id="categories">
      <category-item v-for="(category, index) in categories"
          v-bind:key="index"
          v-bind:categoryItem="category"
          v-bind:activeCategory='activeCategoryName'>
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
      activeCategoryName : '',
    };
  },
  methods:{
    addToCategoryArray(category){
      let categoryItem = category.toUpperCase();
      if(this.categories.includes(categoryItem)) {
        alert(`Already added a category called ${categoryItem}`)
        return;
      }
      
      this.$nextTick(function(){
        this.categories.push(categoryItem)
      });
      
      this.EventBus.$emit('forcedBlur');
    },
    setActiveCategory(categoryName){
      console.log("Update Active Category")
      this.activeCategoryName = this.categories[this.categories.indexOf(categoryName)];
      console.log(this.activeCategoryName)
      this.EventBus.$emit('pass-todo-data', this.activeCategoryName);
    }
  },
  watch: {
    "categories" : function (){
      if(this.categories){
        this.EventBus.$emit('check-categories-array', true);
        if(this.categories.length == 1)
        {
          this.$nextTick(function(){
              console.log(`Last item = ${this.categories[this.categories.length - 1]}`)
              this.EventBus.$emit('active-event', this.categories[this.categories.length - 1])})
        }
        else if(this.categories.length >= 1)
        {
          this.$nextTick(function(){
            this.EventBus.$emit('reset-category-div')
            this.$nextTick(function(){
              console.log(`Last item = ${this.categories[this.categories.length - 1]}`)
              this.EventBus.$emit('active-event', this.categories[this.categories.length - 1])})
              this.setActiveCategory(this.categories[this.categories.length - 1])
          })
        }
      }
      else{
        this.EventBus.$emit('check-categories-array', false);
      }
    }
  },
  mounted (){
    // this.EventBus.$on()
    this.hasCategories = false;
    this.EventBus.$on('set-active-category', this.setActiveCategory);
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