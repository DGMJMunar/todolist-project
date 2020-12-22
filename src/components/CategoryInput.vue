<template>
  <input
    type="text"
    placeholder="ADD CATEGORY"
    onfocus="this.placeholder = ''"
    onblur="this.placeholder='ADD CATERGORY'"
    ref = "categoryItemInput"
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
        this.$refs.categoryItemInput.blur();
      }
    },
    emitToParent() {
      if (this.text == "") return;

      this.$emit("addCategory", this.text);
      this.text = "";
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  input[type=text] {
    margin-left: 15px;
    width: 80%;
    background-color: none;
    background: transparent;
    border-style: none;
    font-weight: 900;
    font-size: 25px;
    color: white;
    margin-left: 55px;
    text-transform: uppercase;
  }
  input[type=text]:focus {
    background-color: black;
    
  }
  ::placeholder{
    color:#727272;
    margin-left: 55px;
  }
</style>
