<template >
    <g id="project_ring"></g>
</template>
<script>
import {defineComponent} from "vue";
import * as d3 from "d3";
import project_list from '../assets/project_list.json';

export default defineComponent({
    data() {
        return {
            radius: 250,
            bar_width: 5,
            bar_max_height: 70,
            translate:{
                x: 500,
                y: 300
            },
        }
    },
    methods: {
        drawRing(){
            let g = d3.select('#project_ring');
            g.attr("transform", `translate(${this.translate.x} ${this.translate.y})`);
            let yScale = d3.scaleLinear().domain([0, d3.max(project_list, d => d.amount)]).range([0, this.bar_max_height]);

            let degree = 360 / project_list.length;
            g.append('g').selectAll('rect').data(project_list).join('rect')
                .attr('width', this.bar_width)
                .attr('height', d => yScale(d.amount))
                .attr('fill', 'black')
                .attr('transform', (d, i) => {
                    let rotate = degree * i;
                    return `rotate(${rotate}) translate(0 ${this.radius})`;
                });
        }
    },
    mounted() {
        this.drawRing();
    },
})
</script>
<style lang="">
    
</style>