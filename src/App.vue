<!-- <script setup>
// import HelloWorld from './components/HelloWorld.vue'
import test from './components/test.vue';
</script> -->

<template>
  <svg width="300px" height="300px"> 
    <test :color="color" @click="change_color"/>
  </svg>
</template>

<script>
  import test from './components/test.vue'
  export default{
    name: 'App',
    components: {
      test
    },
    data() {
      return {
        color: 'black'
      }
    },
    methods:{
      change_color(){
        this.color = 'red';
        console.log(this.color);
      }
    }
  }
</script>

<style scoped>
  svg{ 
    width: '300px';
    height: '300px';
  }
</style>
