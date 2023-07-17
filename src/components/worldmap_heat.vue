<template>
    <g id="map"></g>
</template>
<script>
import { defineComponent } from "vue";
import mapData from "../assets/map.json"
import country2code from '../assets/country2code.json'
import member_sector2project from '../assets/member_sector2project.json'
import country_development from '../assets/human-development-index-hdi-by-country-2023.json'
import project_list from '../assets/project_list.json'
import crf_country_list from '../assets/crf_country_list.json'
import * as d3 from "d3";
export default defineComponent({
    name: "worldmap_heat",
    data() {
        return {
            translate: {
                x: 200,
                y: 150
            },
            scale: 0.6
        }
    },
    methods: {
        draw_map() {
            // console.log(topojson);
            let map = d3.select('#map');
            let geoData = topojson.feature(mapData, mapData.objects.countries).features;
            console.log(geoData);
            let ignore_code = ['010']
            geoData = geoData.filter(d => {
                let isDraw = true;
                ignore_code.forEach(c => {
                    if (d.id == c) {
                        isDraw = false;
                    }
                });
                return isDraw;
            });
            // geoData.forEach(d => {
            //     if(parseInt(d.id) == 184){
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

            map.attr("width", width)
                .attr("height", height)
                // .attr("transform", `translate(${this.translate.x} ${this.translate.y}) scale(${this.scale})`)
                .selectAll(".country")
                .data(geoData)
                .enter()
                .append("path")
                .classed("country", true)
                .attr("d", path)
                .attr("id", d => 'country-' + parseInt(d.id).toString())
                .attr("fill", "white")
                .attr("opacity", 1)
                .attr("stroke", "black");
            let country_num = [];
            for (let member in member_sector2project) {
                country_num.push([member, member_sector2project[member].amount]);
            }
            let heatScale = d3.scaleLinear().domain([d3.min(country_num, d => d[1]), d3.max(country_num, d => d[1])]).range([0, 1])
            let interpolate = d3.interpolate('gray', 'black');
            // fill country by 金额
            // country_num.forEach(d => {
            //     if(d[0] != 'Multicountry'){
            //         let code = country2code[d[0]].toString();
            //         d3.select(`#country-${code}`).attr('fill', interpolate(heatScale(d[1]))).attr("opacity", 1);
            //     }
            // });
            //fill country by 发展水平
            // let develop_degree_color = {};
            // let category_num = 0;
            // // let color_scheme = d3.schemeCategory10;
            // let color_scheme = ['#737373', '#595959', '#333333', '#1a1a1a', '#000000'];
            // // d3.select('#country-184').attr('stroke', 'red');
            // country_development.forEach(d => {
            //     let cur_category = d.hdiTier;
            //     if (!(cur_category in develop_degree_color)) {
            //         develop_degree_color[cur_category] = color_scheme[category_num];
            //         category_num += 1;
            //     }
            //     if (d.country in country2code) {
            //         d3.select(`#country-${country2code[d.country]}`).attr('fill', develop_degree_color[cur_category]).attr("opacity", 1);
            //     } else {
            //         // console.log(d.country);
            //     }
            // });
            // console.log(develop_degree_color);
            //画出项目圆圈
            // let g_circles = map.append('g');
            // for (let i = 0; i < project_list.length; i++) {
            //     let country_code;
            //     if (project_list[i].member in country2code) {
            //         country_code = country2code[project_list[i].member];
            //     } else {
            //         // console.log(project_list[i].member);
            //         continue;
            //     }
            //     for (let j = 0; j < geoData.length; j++) {
            //         if (country_code == parseInt(geoData[j].id)) {
            //             const centroid = path.centroid(geoData[j]);
            //             g_circles
            //                 .append("circle")
            //                 .attr("fill", "none")
            //                 .attr("r", 1 + i / 25)
            //                 .attr("cx", centroid[0])
            //                 .attr("cy", centroid[1])
            //                 .attr("stroke", function () {
            //                     if (project_list[i].status == "Approved") {
            //                         return "green";
            //                     } else if (project_list[i].status == "Proposed") {
            //                         return "blue";
            //                     } else if (project_list[i].status == "On Hold") {
            //                         return "yellow";
            //                     } else if (project_list[i].status = "Terminated / Cancelled") {
            //                         return "red";
            //                     }
            //                 })
            //                 .attr("stroke-width", 1)
            //             break;
            //         }
            //     }
            // }
            // 将地图映射到1维
            // let country_x = {}
            // project_list.forEach(d => {
            //     let country_name = d.member;
            //     if (country_name != 'Multicountry') {
            //         let country_code = country2code[country_name];
            //         if (!(country_name in country_x)) {
            //             for (let i = 0; i < geoData.length; ++i) {
            //                 if(country_code == parseInt(geoData[i].id)){
            //                     const centroid = path.centroid(geoData[i]);
            //                     country_x[country_name] = centroid[0];
            //                 }
            //             }
            //         }
            //     }
            // })
            // let country_x_list = [];
            // for(let country in country_x){
            //     country_x_list.push([country, country_x[country]])
            // }
            // country_x_list.sort((a, b) => a[1] - b[1]);
            //  画出CRF项目圆圈
            let geoData_dict = new Object();
            let country_out = 
            geoData.forEach(d => {
                geoData_dict[parseInt(d.id).toString()] = d;
            });
            let rScale = d3.scaleLinear().domain(d3.extent(crf_country_list, d => d.amount)).range([5, 15]);
            map.append('g').selectAll('circle').data(crf_country_list).join('circle')
                .attr('cx', d => {
                    return path.centroid(geoData_dict[d.code])[0]
                })
                .attr('cy', d => {
                    return path.centroid(geoData_dict[d.code])[1]
                })
                .attr('r', d => {
                    return rScale(d.amount)
                })
                .attr('opacity', 0.4)
                .attr('fill', 'red');

        }
    },
    mounted() {
        this.draw_map();
    },
})
</script>
<style></style>