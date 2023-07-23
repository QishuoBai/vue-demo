<template>
  <svg width="100%" height="100%">
    <worldmap_heat />
    <!-- <region_pie /> -->
    <!-- <scatter /> -->
    <!-- <project_bar /> -->
    <!-- <project_ring /> -->
    <!-- <project_time_line /> -->
    <!-- <timeline_scatter /> -->
    <!-- <country_upline /> -->
    <!-- <crf_project /> -->
  </svg>
</template>

<script>
import worldmap_heat from './components/worldmap_heat.vue';
import region_pie from './components/region_pie.vue';
import scatter from './components/scatter.vue';
import project_bar from './components/project_bar.vue';
import project_ring from './components/project_ring.vue';
import project_time_line from './components/project_time_line.vue';
import timeline_scatter from './components/timeline_scatter.vue';
import country_upline from './components/country_upline.vue';
import crf_project from './components/crf_project.vue';
export default {
  name: 'App',
  components: { worldmap_heat, region_pie, scatter, project_bar, project_ring, project_time_line, timeline_scatter, country_upline, crf_project},
  data() {
    return {

    }
  },
  methods: {

  }
}
</script>

<style></style>
