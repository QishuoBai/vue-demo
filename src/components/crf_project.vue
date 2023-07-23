<template>
    <g id="crf_project">
    </g>
</template>
<script>
import { defineComponent } from 'vue';
import * as d3 from "d3";

export default defineComponent({
    data() {
        return {
            
        }
    },
    methods: {
        drawCircle(){
            let g = d3.select('#crf_project');
            g.append('rect')
                .attr('x', 30)
                .attr('y', 30)
                .attr('width', 100)
                .attr('height', 30)
                .on('click', (event)=>{
                    d3.select(event.target).transition().duration(3000)
                        .attr('x', 130)
                        .attr('width', 0);
                });
        }
    },
    mounted() {
        this.drawCircle();
    },
})
</script>
<style lang="">
    
</style>