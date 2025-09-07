<template>
    <div class="Sim-Header">
        <div class="header-left">
            <slot name="header-left"></slot>
            </div>
        <div class="header-center">
            <span class="pre-month" @click="$emit('goPrev')" >{{ leftArrow }}</span>
            <span class="title">{{ title }}</span>
            <span class="next-month" @click="$emit('goNext')" >{{ rightArrow }}</span>
            </div>
        <div class="header-right">
            <slot name="header-right"></slot>
            </div>
        </div>
</template>

<script>

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