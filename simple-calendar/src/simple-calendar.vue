<template>
<SimHeader :currentMonth="currentMonth" :locale="locale" @changeMonth="changeMonth"></SimHeader>
</template>

<script>
import SimHeader from './components/Sim-Header.vue';
import moment from 'moment';

export default {
  props:{
    locale:{
      type:String,
      default:'en'
    }
  },
  components:{
      SimHeader
  },
  data(){
    return{
      currentMonth:moment().startOf('month'),
    }
  },
  methods:{
    changeMonth(newMonth){
      this.currentMonth=newMonth;
      console.log('Month changed to:', this.currentMonth.format('MMMM YYYY') );
    }
  }
}
</script>

<style>

</style>
