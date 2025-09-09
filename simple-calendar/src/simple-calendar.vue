<template>
  <div  @keydown="keydownHandler" tabindex="0">
<SimHeader :currentMonth="currentMonth" :locale="locale" @goPrev="goPrev" @goNext="goNext">
  <template #header-left>
    <slot></slot>
  </template>
  <template #header-right>
    <slot></slot>
  </template>
</SimHeader>

<SimBg ref="bg" :locale="locale" :currentMonth="currentMonth"></SimBg>

  </div>
</template>

<script>
import SimHeader from './components/Sim-Header.vue';
import SimBg from './components/Sim-Background.vue';
import moment from 'moment';

export default {
  props:{
    locale:{
      type:String,
      default:'en'
    }
  },
  components:{
      SimHeader,
      SimBg,
  },
  data(){
    return{
      currentMonth:moment().startOf('month'),
    }
  },
  methods:{
    changeMonth(newMonth){
      this.currentMonth=newMonth;
    },
    goPrev(){
        var newMonth=moment(this.currentMonth).subtract(1,'months').startOf('month');
        this.changeMonth(newMonth);
        
    },
    goNext(){
        var newMonth=moment(this.currentMonth).add(1,'months').startOf('month');
        this.changeMonth(newMonth);
    },
    keydownHandler(event){
      if(event.ctrlKey&&event.key=='ArrowLeft'){
        this.goPrev();
      }else if(event.ctrlKey&&event.key=='ArrowRight'){
        this.goNext();
      }else if(event.key=="Tab"){
        this.$refs.bg.handleTab();
      }
    }
  },
}
</script>

<style>

</style>
