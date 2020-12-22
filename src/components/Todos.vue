




<template>
<div>
    <hr>
    <p>{{todoItem}}</p>
</div>
  
</template>

<script>
export default {
    props: {
        todoItem: {
            type : String,
            required: true
        }
    }
}
</script>

<style>
hr{
    border: 1px solid grey;
}
p{
    color: #a0a0a0;
}
</style>