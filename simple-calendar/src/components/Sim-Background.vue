<template>
<div>
<div class="Sim-Background">
    <div class="bg-left">
        <slot name="bg-left"></slot>
        </div>
    <div class="bg-center">
        <div class="week-list">
            <span v-for="(weekday,index) in weekdays" :key="index" class="week">
                {{weekday}}
            </span>
        </div>
        </div>
    <div class="bg-right">
        <slot name="bg-right"></slot>
        </div>    
    </div>
<div class="Sim-Background">
    <div class="bg-left">
        <slot name="bg-left"></slot>
        </div>
    <div class="bg-center">
        <div class="day-list">
            <div v-for="week in 7" :key="week" class="day-col">
                <span v-for="day in 6" :key="day" :class="{'day-row':!isCurDay(week,day),'cur-day':isCurDay(week,day)}">
                    <p v-if="createDay(week,day)" class="date">{{ createDay(week,day) }}</p>
                </span>
            </div>
            </div>
        </div>
    <div class="bg-right">
        <slot name="bg-right"></slot>
        </div>    
    </div>
</div>
</template>

<script>
import moment from 'moment';

export default{
    props:{
        locale:{},
        currentMonth:{}
    },
    data(){
        return {
            
        }
    },
    computed:{
        weekdays(){
            return moment().locale(this.locale).localeData().weekdaysShort();
        },
        dayInCurMonth(){
            return moment(this.currentMonth).daysInMonth();
        },
        firstDayOfMonth(){
            return moment(this.currentMonth).startOf('month').day();
        },
        currentDay(){
            return moment().date();
        },
    },
    methods:{
        createDay(week,day){
            const index=(week-1)+day*7-7;
            if(index<this.firstDayOfMonth||index>this.firstDayOfMonth+this.dayInCurMonth-1) return 0;
            else return index-this.firstDayOfMonth+1;
        },
        isCurDay(week,day){
            return this.createDay(week,day)===this.currentDay&&moment().isSame(this.currentMonth,'month');
        }
    }
}
</script>

<style scoped>
.Sim-Background{
    display: flex;
    align-items: center;
}
.bg-left{
    flex:1;
    background-color: lightgray;
}
.bg-center{
    flex:3;
    background-color: lightblue;
    text-align: center;
}
.bg-right{
    flex:1;
    background-color: lightgray;
}
.week-list{
    display: flex;
    justify-content: space-around;
    margin-top: 2%;
}
.week{
    flex:1;
}
.day-list{
    display: flex;
    height: 500px;
}
.day-col{
    flex:1;
    display: flex;
    flex-direction: column;
}
.day-row{
    flex:1;
    border: 1px solid white;
}
.cur-day{
    flex:1;
    border: 1px solid white;
    background: rgba(218, 233, 4, 0.642);
}
.date{
    position: relative;
    left:-35%;
    top: -20%;
}
</style>
