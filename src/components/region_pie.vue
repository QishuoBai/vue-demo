<template>
    <g id="region_pie"></g>
</template>
<script>
import { defineComponent } from "vue";
import * as d3 from "d3";
import member_sector2project from '../assets/member_sector2project.json'
export default defineComponent({
    data() {
        return {
            
        }
    },
    methods: {
        drawPie(){
            let region_num = {};
            for(let member in member_sector2project){
                if(member_sector2project[member].region in region_num){
                    region_num[member_sector2project[member].region] += member_sector2project[member].amount;
                }else{
                    region_num[member_sector2project[member].region] = member_sector2project[member].amount;
                }
            }
            let region_num_list = []
            for(let region in region_num){
                region_num_list.push([region, region_num[region]]);
            }
            let arc = d3.arc()
                .innerRadius(230)
                .outerRadius(250);

            // 資料轉為角度資料
            let arcs = d3.pie().padAngle(0.01)(region_num_list.map(d => d[1]));
            console.log(arcs);

            d3.select('#region_pie')
                .selectAll('path')
                .data(arcs)
                .enter()
                .append('path')
                .attr('d', d => arc(d))
                .attr('stroke', 'white');
            d3.select('#region_pie').attr("transform", "translate(300 300)");
        }
    },
    mounted() {
        this.drawPie();
    },
})
</script>
<style>
    
</style>