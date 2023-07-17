<template>
    <g id="country_upline"></g>
</template>
<script>
import {defineComponent} from "vue";
import * as d3 from "d3";
import country_timeline from "../assets/country_timeline.json"
export default defineComponent({
    data() {
        return {
            translate:{
                x: 50,
                y: 30
            },
            width: 1000,
            height: 500,
        }
    },
    methods: {
        draw_upline(y_type){
            let g = d3.select('#country_upline');
            g.attr('transform', `translate(${this.translate.x} ${this.translate.y})`);
            g.append('g').append('text').text(y_type)
                .attr('text-anchor', 'middle')
                .attr('dy', -5)
                .style('font-size', '12px');
            let xdata = [];
            //generate xdata
            for(let year = 2016; year < 2024; ++year){
                for(let month = 1; month < 13; ++month){
                    xdata.push(year.toString() + '-' + month.toString());
                }
            }
            let xScale = d3.scaleBand().domain(xdata).range([0, this.width]);
            let yScale = d3.scaleLinear().domain([0, d3.max(country_timeline, d => d[y_type])]).range([this.height, 0]);
            let line = d3.line()
                .x(d => xScale(d.time))
                .y(d => yScale(d['acc_' + y_type]))
                .curve(d3.curveBumpX);
            g.append('g').selectAll('path').data(country_timeline).join('path')
                .attr('d', d => line(d.timeline))
                .attr('fill', 'none')
                .attr('stroke', 'black')
                .attr('transform', `translate(${xScale.bandwidth()/2} 0)`);
            g.append('g').call(new d3.axisLeft().scale(yScale));
            g.append('g').call(new d3.axisBottom().scale(xScale)).attr('transform', `translate(0 ${this.height})`)
                .selectAll('text')
                .attr('transform', 'rotate(90) translate(5 -12)')
                .attr('text-anchor', 'start');
            g.append('g').selectAll('text').data(country_timeline).join('text')
                .attr('x', xScale('2023-12') + 8)
                .attr('y', d => yScale(d[y_type]))
                .attr('text-anchor', 'start')
                .attr('alignment-baseline', 'middle')
                .text(d => d.country)
                .style('font-size', '10px');

        }
    },
    mounted(){
        // this.draw_upline('project_num');
        this.draw_upline('amount');
    }
})
</script>
<style lang="">
    
</style>