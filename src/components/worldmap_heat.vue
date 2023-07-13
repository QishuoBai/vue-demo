<template>
    <g id="map"></g>
</template>
<script>
import { defineComponent } from "vue";
import mapData from "../assets/map.json"
import country2code from '../assets/country2code.json'
import member_sector2project from '../assets/member_sector2project.json'
import * as d3 from "d3";
export default defineComponent({
    name: "worldmap_heat",
    data() {
        return {
            
        }
    },
    methods: {
        draw_map(){
            // console.log(topojson);
            let geoData = topojson.feature(mapData, mapData.objects.countries).features; 
            let ignore_code = ['010']
            geoData = geoData.filter(d => {
                let isDraw = true; 
                ignore_code.forEach(c => {
                    if(d.id == c){
                        isDraw = false;
                    }
                });
                return isDraw;
            });
            // geoData.forEach(d => {
            //     if(parseInt(d.id) == 31){
            //         console.log(d);
            //     }
            // })          
            
            // console.log(geoData);
            let width = 960;
            let height = 600;
            
            let projection = d3.geoMercator()
                .scale(125)
                .translate([width / 2, height / 1.4])
            
            let path = d3.geoPath()
                .projection(projection)
            
            d3.select("#map")
                .attr("width", width)
                .attr("height", height)
                .selectAll(".country")
                .data(geoData)
                .enter()
                .append("path")
                .classed("country", true)
                .attr("d", path)
                .attr("id", d => 'country-' + parseInt(d.id).toString())
                .attr("fill", "lightgray")
                .attr("opacity", 1)
                .attr("stroke", "white");
            let country_num = [];
            for(let member in member_sector2project){
                country_num.push([member, member_sector2project[member].amount]);
            }
            let heatScale = d3.scaleLinear().domain([d3.min(country_num, d => d[1]), d3.max(country_num, d => d[1])]).range([0, 1])
            let interpolate = d3.interpolate('gray', 'black');
            country_num.forEach(d => {
                if(d[0] != 'Multicountry'){
                    let code = country2code[d[0]].toString();
                    d3.select(`#country-${code}`).attr('fill', interpolate(heatScale(d[1]))).attr("opacity", 1);
                }
            })
        }
    },
    mounted() {
        this.draw_map();
    },
})
</script>
<style>
    
</style>