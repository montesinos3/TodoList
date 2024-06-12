<script setup>
import todos from './components/todoList.vue'
</script>


<template>
  <todos></todos>
</template>


<style>
body{
  margin: 0px;
  padding: 0px;
  width: 100%;
  height: 100%;
  background-image: linear-gradient(180deg, rgb(119, 37, 251), rgb(134, 59, 255), rgb(146, 78, 255), rgb(158, 98, 255), rgb(168, 114, 255));
  background-size: 500%;
  animation: anim 10s ease infinite;
}

@keyframes anim{
  0%{
    background-position: 0% 50%;
  }
  50%{
    background-position: 100% 50%;
  }
  100%{
    background-position: 0% 50%;
  }
}

div{
  margin: 0%;
  padding: 0%;
}
</style>
