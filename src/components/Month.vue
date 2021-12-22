<template>
  <div class="month">
    <div class="day-names">
      <div class="day-name" v-for="d in Array(7).fill().map((x,i)=>i)" :key="d">
        {{dayNames[d]}}
      </div>
    </div>
    <div class="week" v-for="(week, index) of weeks" :key="index">
      <div class="day" v-for="date in week.days" :key="date.day">
        <span class="day-number" v-if="date.day">
          {{date.day}}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable no-debugger */
import * as dateFns from 'date-fns';
import {es} from 'date-fns/esm/locale'

export default {
  name: 'Month',
  props: {
    year: {
      type: Number,
      default: 2022
    },
    month: {
      type: Number,
      default: 1
    },
  },
  data() {
    return {
      daysInMonth: Number,
      weeks: [],
    }
  },
  mounted: function() {
    this.daysInMonth = dateFns.getDaysInMonth(dateFns.toDate(this.year, this.month));
    for (let day = 1; day <= this.daysInMonth; day++) {
      const dayDate = new Date(this.year, this.month - 1, day);
      const weekInMonth = dateFns.getWeekOfMonth(dayDate, {locale: 'es', weekStartsOn: 1}) - 1;
      let week = this.weeks[weekInMonth];

      if (!week) {
        week = {
          days: Array(7).fill().map(()=> { return {}})
        }
        this.weeks.push(week);
      }

      let dayInWeekValue = dateFns.getDay(dayDate);
      dayInWeekValue = dayInWeekValue === 0 ? 6 : dayInWeekValue - 1;
      const dayInWeek = week.days[dayInWeekValue];
      dayInWeek.date = dayDate;
      dayInWeek.day = day;
      dayInWeek.name = dateFns.format(dayDate, 'EEEE', {locale: es});
    }
  },
  computed: {
    dayNames() {
      return ['Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado', 'Domingo',];
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.month {
  max-width: 896px;
  margin: 64px auto 0 auto;
}
.day-names {
  display: grid;
  grid-template-columns: repeat(7, 1fr); 
  gap: 0px 0px;
}
.day-name {
  width: 146px;
}
.week {
  display: grid;
  grid-template-columns: repeat(7, 1fr); 
  gap: 0px 0px;
}

.day {
  border: solid 1px #ccc;
  width: 128px;
  height: 84px;
  display: flex;
  align-items: flex-start;
  justify-content: flex-end;
  padding: 4px 8px 8px 8px;
}

.day-number {
  font-size: 1.5em;
  font-weight: bold;
}
  
</style>
