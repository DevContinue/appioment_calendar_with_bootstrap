<template>
    <div id="calender-week">
          <div class="card-group">
            <!-- Anfang: Template für die Calendar-Day-Component -->
            <CalendarDay v-for="day in calendarWeekData" :key="day.id" :day="day" />
            <!-- Ende: Template für die Calendar-Day-Component -->
          </div>
        </div>

</template>

<script>
import CalendarDay from './CalendarDay.vue';
import store from "../store.js";

export default {
  name: "CalendarWeek",
  components: {
    CalendarDay,
  },
  data() {
    return {
      calendarWeekData: store.state.calendarWeekData,
    };
  },
};
</script>

<style lang="sass" scoped>
@import "@/scss/variables.scss"


  

</style>