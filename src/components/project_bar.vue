<template>
    <g id="bar">
        <text>{{ project_detail }}</text>
    </g>
</template>
<script>
import {defineComponent} from "vue";
import * as d3 from "d3";
import project_list from '../assets/project_list.json';

export default defineComponent({
    data() {
        return {
            width: 1200,
            height: 70,
            translate:{
                x: 50,
                y: 50
            },
            project_detail: '',
        }
    },
    methods: {
        drawBar(){
            let g = d3.select('#bar');
            // console.log(project_list.length);
            g.attr("transform", `translate(${this.translate.x} ${this.translate.y})`);
            let xScale = d3.scaleBand().domain(project_list.map(d => d.name)).range([0, this.width]).padding(0.1);
            let yScale = d3.scaleLinear().domain([0, d3.max(project_list, d => d.amount)]).range([0, this.height]);
            let xAxis = d3.axisBottom().scale(xScale);
            let yAxis = d3.axisLeft().scale(yScale);
            // g.append('g').call(xAxis).attr('transform', `translate(0, ${this.height})`).selectAll('text').remove();
            // g.append('g').call(yAxis);

            g.append('g').selectAll('rect').data(project_list).join('rect')
                .attr('x', d => xScale(d.name))
                .attr('y', d => this.height - yScale(d.amount))
                .attr('width', xScale.bandwidth())
                .attr('height', d => yScale(d.amount))
                .on('mouseover', d => {
                    let cur_project = d3.select(d.target).data()[0];
                    this.project_detail = cur_project.name + '----' + cur_project.amount;
                })
                .on('mouseout', ()=>{
                    this.project_detail = '';
                });

        }
    },
    mounted() {
        this.drawBar();
    },
})
</script>
<style lang="">
    
</style>