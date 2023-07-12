<template>
    <g id="test-bar" v-bind:class="class_name">

    </g>
</template>
<script>
import { defineComponent } from 'vue'
import * as d3 from 'd3'
import data from '../assets/data.json'
export default defineComponent({
    name: "test",
    components: {},
    data() {
        return {
            class_name: 'normal',
            rect: null
        }
    },
    props:{
        color:{
            type: String,
            defalut: 'green'
        }
    },
    methods: {
        drawBar(){
            this.rect = d3.select('#test-bar').append('rect').attr('height', '10px').attr('width', '10px').attr('fill', this.color);
            this.rect.on('mouseover', ()=>{
                this.class_name = 'hover';
            }).on('mouseout', () =>{
                this.class_name = 'normal';
            })
            
        }
    },
    watch:{
        color(color, oldcolor){
            this.rect.attr('fill', color);
        }
    },
    mounted() {
        console.log(this.color);
        console.log(data);
        this.drawBar();
    },
})
</script>
<style>
    .normal{
        opacity: 1;
    }
    .hover{
        opacity: 0.5;
    }
</style>