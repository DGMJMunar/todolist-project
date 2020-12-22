<template>
  <div id = "categoryItemText" class="border border-light">
    {{categoryItem}}
  </div>
</template>

<script>
import Todos from "./Todos.vue";
export default {
  props: {
    categoryItem: {
      type: Object,
      required: true,
      // default{

      // }
    },
  },
};
</script>

<style>
#categoryItemText {
  color:white;
  font-weight: 900;
  font-size: 30px;
  text-transform: uppercase;
  text-align: center;
  /* background-color:red; */
}
</style>