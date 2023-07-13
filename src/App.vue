<template>
  <svg width="100%" height="100%">
    <!-- <worldmap_heat/> -->
    <!-- <region_pie /> -->
    <scatter />
  </svg>
</template>

<script>
  import worldmap_heat from './components/worldmap_heat.vue';
  import region_pie from './components/region_pie.vue';
  import scatter from './components/scatter.vue';
  export default{
    name: 'App',
    components: {worldmap_heat, region_pie, scatter},
    data() {
      return {
        
      }
    },
    methods:{
      
    }
  }
</script>

<style>
</style>
