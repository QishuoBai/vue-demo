<template>
    <g id="test-bar" v-bind:class="class_name">

    </g>
</template>
<script>
import { defineComponent } from 'vue'
import * as d3 from 'd3'
export default defineComponent({
    name: "test",
    components: {},
    data() {
        return {
            class_name: 'normal'
        }
    },
    methods: {
        drawBar(){
            let rect = d3.select('#test-bar').append('rect').attr('height', '10px').attr('width', '10px');
            rect.on('mouseover', ()=>{
                this.class_name = 'hover';
            }).on('mouseout', () =>{
                this.class_name = 'normal';
            })
            
        }
    },
    mounted() {
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