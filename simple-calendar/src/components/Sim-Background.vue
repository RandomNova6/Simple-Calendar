<template>
<div>
<div class="Sim-Background-Head">
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
<div class="Sim-Background-Body">
    <div class="bg-left">
        <slot name="bg-left"></slot>
        </div>
    <div class="bg-center">
        <div class="day-list">
            <div v-for="week in 7" :key="week" class="day-col">
                <span v-for="day in 6" :key="day" :class="{'day-row':!isCurDay(week,day),'cur-day':isCurDay(week,day),'leftdown':week==1&&day==6,'rightdown':week==7&&day==6}">
                    <div :class="{'event':true,'selected':selectedNum===(week-1)+(day-1)*7}" v-if="createDay(week,day)" @click="selecteCell(week,day)">
                        <p  class="date">{{ calendarCells[(week-1)+(day-1)*7].date }}</p>
                        <todo :ref="'child-' + ((week-1)+(day-1)*7)" :date="createDay(week,day)" :currentMonth="currentMonth"></todo>
                    </div>
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
import todo from './Sim-Todo.vue';

export default{
    components:{
        todo,
    },
    props:{
        locale:{},
        currentMonth:{}
    },
    data() {
        return {
            calendarCells:Array(42).fill().map((_, index) => ({
                id: index,
                date: null,
                isCurrentMonth: false,
            })),
            selectedNum:-1,
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
            const index=(week-1)+(day-1)*7;
            if(index<this.firstDayOfMonth||index>this.firstDayOfMonth+this.dayInCurMonth-1) return 0;
            else {
                this.calendarCells[index].date=index-this.firstDayOfMonth+1;
                this.calendarCells[index].isCurrentMonth=true;
                return index-this.firstDayOfMonth+1;
            }
        },
        isCurDay(week,day){
            return this.createDay(week,day)===this.currentDay&&moment().isSame(this.currentMonth,'month');
        },
        activateTodo(week,day){
            this.$refs[`child-${(week-1) + (day-1)*7}`][0].setTodo(true);
        },
        selecteCell(week,day){
            this.activateTodo(week,day);
            if(this.selectedNum!==(week-1)+(day-1)*7)this.selectedNum=(week-1)+(day-1)*7;
            else this.selectedNum=-1;
        },
    }
}
</script>

<style scoped>
.Sim-Background-Head{
    margin-top:-2pt;
    display: flex;
    align-items: center;
}
.Sim-Background-Body{
    margin-top:-2pt;
    display: flex;
    align-items: center;
}
.bg-left{
    flex:1;
    background-color: lightgray;
}
.bg-center{
    flex:3;
    background-color: transparent;
    border: none;
    text-align: center;
}
.bg-right{
    flex:1;
    background-color: lightgray;
}
.week-list{
    display: flex;
    justify-content: space-around;
    background-color: lightblue;
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
    position: relative;
}
.day-row{
    flex:1;
    border: 1px solid rgba(77, 80, 86, 0.694);
    position: relative;
    background-color: lightblue
}
.leftdown{
    border-radius: 0 0 0 10pt;
}
.rightdown{
    border-radius: 0 0 10pt 0;
}
.cur-day{
    flex:1;
    border: 1px solid rgba(77, 80, 86, 0.694);
    background: rgba(218, 233, 4, 0.642);
    position: relative;
}
.event{
    cursor: pointer;
    position: relative;
    height: 100%;
    width: 100%;
}
.date{
    position:absolute;
    left:5%;
    top: -22%;
}
.selected{
    border:3px dashed rgb(255, 179, 0);
    box-sizing: border-box;
}
</style>
