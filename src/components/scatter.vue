<template>
    <g id="scatter"></g>
</template>
<script>
import { defineComponent } from 'vue';
import * as d3 from "d3";
import project_list from '../assets/project_list.json';

export default defineComponent({
    data() {
        return {
            translate:{
                x: 50,
                y: 50
            },
            width: 1200,
            height: 600,
            xdata: [],
            ydata: ["2016", "2017", "2018", "2019", "2020", "2021", "2022", "2023"],
            item_list: []
        }
    },
    methods:{

        unique_list(list){
            const res = new Map();
            return list.filter((item) => !res.has(item) && res.set(item, 1));
        },
        get_data(attribute){
            project_list.forEach(project => {
                this.xdata.push(project[attribute]);
                if(project.approval_year != "—"){
                    this.item_list.push({
                        x: project[attribute],
                        y: project.approval_year,
                        value: project.amount
                    })
                }
            });
            this.xdata = this.unique_list(this.xdata).sort();
            let item_list_sum = {};
            this.item_list.forEach(item => {
                if(item.x in item_list_sum){
                    if(item.y in item_list_sum[item.x]){
                        item_list_sum[item.x][item.y] += item.value;
                    }else{
                        item_list_sum[item.x][item.y] = item.value;
                    }
                }else{
                    item_list_sum[item.x] = {};
                    item_list_sum[item.x][item.y] = item.value;
                }
            });
            this.item_list = [];
            for(let x in item_list_sum){
                for(let y in item_list_sum[x]){
                    this.item_list.push({
                        x: x,
                        y: y,
                        value: item_list_sum[x][y]
                    })
                }
            };
        },
        drawScatter(){
            let g = d3.select('#scatter');
            console.log(this.item_list);
            g.attr("transform", `translate(${this.translate.x} ${this.translate.y})`);
            let xScale = d3.scaleBand().domain(this.xdata).range([0, this.width]);
            let yScale = d3.scaleBand().domain(this.ydata).range([0, this.height]);
            let xAxis = d3.axisTop().scale(xScale);
            let yAxis = d3.axisLeft().scale(yScale);
            g.append('g').call(xAxis).selectAll('text').attr('transform', 'translate(0 -30) rotate(-60)');
            g.append('g').call(yAxis);
            let radiusScale = d3.scaleLinear()
                .domain([d3.min(this.item_list, d => d.value), d3.max(this.item_list, d => d.value)])
                .range([5, 50]);
            g.append('g').selectAll('circle').data(this.item_list).join('circle')
                .attr('cx', d => xScale(d.x))
                .attr('cy', d => yScale(d.y))
                .attr('r', d => radiusScale(d.value))
                .attr('opacity', 0.4);
        }
    },
    mounted() {
        // this.get_data('member');
        this.get_data('sector');
        this.drawScatter();
    },
})
</script>
<style>
    
</style>