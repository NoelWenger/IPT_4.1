<script setup lang="ts">
  import {ref} from "vue";

  let name = ref("???")
  function generate(){
    name.value = "my starup"
    console.log("Function clicked")
  }

</script>

<template>
  <h1>Dein Startup heisst</h1>
  <div>{{name}}</div>
  <button @click="generate">Name generieren</button>

</template>

<style scoped>


</style>