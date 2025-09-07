<template>
    <div class="Sim-Header" @keydown.ctrl.left="goPrev" @keydown.ctrl.right="goNext" tabindex="0">
        <div class="header-left">
            <slot></slot>
            </div>
        <div class="header-center">
            <span class="pre-month" @click="goPrev" >{{ leftArrow }}</span>
            <span class="title">{{ title }}</span>
            <span class="next-month" @click="goNext" >{{ rightArrow }}</span>
            </div>
        <div class="header-right">
            <slot></slot>
            </div>
        </div>
</template>

<script>

import moment from 'moment';

let debug=true;
export default{
    props:{
        currentMonth:{},
        locale:{}
    },
    data(){
        return{
            leftArrow:'<',
            rightArrow:'>',
        }
    },
    methods:{
        goPrev(){
           if(debug) console.log('goPrev')
            var newMonth=moment(this.currentMonth).subtract(1,'months').startOf('month');
            this.$emit('changeMonth',newMonth)
           
        },
        goNext(){
           if(debug) console.log('goNext')
            var newMonth=moment(this.currentMonth).add(1,'months').startOf('month');
            this.$emit('changeMonth',newMonth)
        }
    },
    computed:{
         title(){
             if(!this.currentMonth) return;
             return this.currentMonth.locale(this.locale).format('MMMM YYYY')
        }
    }
}
</script>

<style scoped>
.Sim-Header{
    display: flex;
    align-items: center;
}
.header-left{
    flex:1;
    background-color: lightgray;
}
.header-center{
    flex:3;
    background-color: lightblue;
    text-align: center;
}
.header-right{
    flex:1;
    background-color: lightgray;
}
.pre-month{
    cursor: pointer;
    position: absolute;
    left: 30%;
}
.next-month{
    cursor: pointer;
    position: absolute;
    right: 30%;
}
.title{
    margin: 0 10px;
}
</style>