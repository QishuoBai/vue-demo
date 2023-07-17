<template>
    <g id="timeline_scatter"></g>
</template>
<script>
import { defineComponent } from 'vue';
import * as d3 from "d3";
import project_list from "../assets/project_list_new.json"
// import country_development_map from "../assets/human-development-index-hdi-by-country-2023.json"
export default defineComponent({
    data() {
        return {
            translate:{
                x: 15,
                y: 500
            },
            width: 1200,
            height: 600,
            circle_radius: 5,
        }
    },
    computed:{
        circle_padding(){
            return this.circle_radius * 2 + 2;
        }
    },
    methods:{
        draw_timeline_scatter(){
            let g = d3.select('#timeline_scatter').attr('transform', `translate(${this.translate.x} ${this.translate.y})`);
            let year_list = [];
            let nation_region_list = [];
            let sector_type_list = [];
            project_list.forEach(d => {
                year_list.push(d.p_year);
                nation_region_list.push(d.p_region);
                sector_type_list.push(d.p_sector)
            });
            
            year_list = [...new Set(year_list)].sort();
            nation_region_list = [...new Set(nation_region_list)].sort();
            sector_type_list = [...new Set(sector_type_list)].sort();
            let xdata = [];
            year_list.forEach(year => {
                nation_region_list.forEach(region => {
                    xdata.push(year + '-' + region);
                })
            });
            // console.log(xdata)
            let xScale = d3.scaleBand().domain(xdata).range([0, this.width]);
            // console.log(xdata);
            let xdata_num = {}
            xdata.forEach(d => {
                xdata_num[d] = 0;
            })
            let sector_color_scheme = d3.schemeCategory10;
            let sector2color = {};
            sector_type_list.forEach((d, i) => {
                sector2color[d] = sector_color_scheme[i];
            });
            //sort project by p_sector
            let secotr_project_list = {};
            project_list.forEach(d => {
                if(!(d.p_sector in secotr_project_list)){
                    secotr_project_list[d.p_sector] = []
                }
                secotr_project_list[d.p_sector].push(d);
            })
            let project_list_sorted_by_sector = [];
            for(let sector in secotr_project_list){
                secotr_project_list[sector].forEach(d => {
                    project_list_sorted_by_sector.push(d)
                })
            }
            //draw scatter
            g.append('g').selectAll('circle').data(project_list_sorted_by_sector).join('circle')
                .attr('r', this.circle_radius)
                .attr('fill', d => sector2color[d.p_sector])
                .attr('cx', d => xScale(d.p_year + '-' + d.p_region) + xScale.bandwidth()/2)
                .attr('cy', d => {
                    let cur_x = d.p_year + '-' + d.p_region;
                    xdata_num[cur_x] += 1;
                    return - xdata_num[cur_x] * this.circle_padding - 4;
                });
            let xAxis = d3.axisBottom().scale(xScale).tickSize(2);

            g.append('g').call(xAxis)
                .selectAll('text')
                .attr('transform', 'rotate(90) translate(3 -9)')
                .attr('text-anchor', 'start');

            g.append('g').selectAll('rect').data(year_list).join('rect')
                .attr('height', this.height)
                .attr('width', xScale.bandwidth() * nation_region_list.length)
                .attr('x', d => {
                    return xScale(d + '-' + 'Africa')
                })
                .attr('transform', `translate(0 ${-this.height})`)
                .attr('fill', 'none')
                .attr('stroke', 'black');
        }
    },
    mounted(){
        this.draw_timeline_scatter();
    }
})
</script>
<style lang="">
    
</style>