<template>
  <input
    type="text"
    placeholder="ADD ITEM"
    onfocus="this.placeholder = ''"
    onblur="this.placeholder='ADD ITEM'"
    ref = "todoItemInput"
    v-model="text"
    v-on:keyup="onKeyPress"
  />
</template>

<script>
export default {
  data() {
    return {
      text: "",
    };
  },
  methods: {
    onKeyPress: function (key) {
      if (key.keyCode === 13) {
        this.emitToParent();
        this.$refs.todoItemInput.blur();
      }
    },
    emitToParent() {
      if (this.text == "") return;

      this.$emit("add-Todo", this.text);
      this.text = "";
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  input[type=text] {
    background-color: none;
    background: transparent;
    border-style: none;
    font-weight: 900;
    font-size: 21px;
    color: white;
  }
  input[type="text"]:focus {
    background-color: black;
    color: white;
  }
  ::placeholder{
    color:#727272;
  }
</style>
