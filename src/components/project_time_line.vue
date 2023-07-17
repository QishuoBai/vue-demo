<template>
    <g id="project_time_line"></g>
</template>
<script>
import {defineComponent} from "vue";
import * as d3 from "d3";

export default defineComponent({
    data() {
        return {
            translate:{
                x: 5,
                y: 300
            },
            width: 1200,
            height: 200,
            ydata: ['status 1', 'status 2', 'status 3'],
            line_num: 288
        }
    },
    methods:{
        generate_random_number(n, m){
            return Math.random()*(m-n) + n;
        },
        draw_project_timeline_h(){
            let g = d3.select('#project_time_line');
            g.attr('transform', `translate(${this.translate.x} ${this.translate.y})`)
            let xScale = d3.scaleLinear().domain([2016, 2023]).range([0, this.width]);
            let yScale = d3.scaleBand().domain(this.ydata).range([0, this.height]);
            const line = d3.line().x(d => xScale(d.time)).y(d => yScale(d.status));
            for(let i = 0; i < this.line_num; ++i){
                 let time1 = this.generate_random_number(2016, 2023);
                 let time2 = this.generate_random_number(time1, Math.min(time1+0.4, 2023));
                 let time3 = this.generate_random_number(time2, Math.min(time2+0.3, 2023));
                 let cur_project = [
                    {status: 'status 1', time: time1},
                    {status: 'status 2', time: time2},
                    {status: 'status 3', time: time3},
                ];
                // console.log(cur_project);
                g.append('path')
                    .attr('d', line(cur_project))
                    .attr('fill', 'none')
                    .attr('stroke', 'black');
            }
        },
        // draw_project_timeline_v(){
        //     let g = d3.select('#project_time_line');
        //     g.attr('transform', `translate(${this.translate.x} ${this.translate.y})`)
        //     let xScale = d3.scaleLinear().domain([2016, 2023]).range([0, this.width]);
        //     let yScale = d3.scaleBand().domain(this.ydata).range([0, this.height]);
        // },
    },
    mounted() {
        this.draw_project_timeline_h();
    },
})
</script>
<style lang="">
    
</style>