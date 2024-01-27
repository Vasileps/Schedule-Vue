<template>
  <div style="margin: 10px;">
    <WeekSchedule :week="weeks.current" :subjects="subjectList" />
    <WeekSchedule :week="weeks.next" :subjects="subjectList" />
  </div>
</template>

<script setup>
import WeekSchedule from '@/components/WeekSchedule.vue';
import { computed, ref } from 'vue'

const semester = ref();
const subjectList = ref();

fetch("./subjects.json")
  .then(response => response.json())
  .then(json => subjectList.value = json);

fetch("./semester.json")
  .then(response => response.json())
  .then(json => semester.value = json);

const weeks = computed(() => {
  let current = {};
  let next = {};

  let today = new Date();
  let day = today.getDay();
  let diff = today.getDate() - day + 1;
  today.setDate(diff);

  current.startDate = today;
  current.endDate = addDays(current.startDate, 6);

  next.startDate = addDays(current.endDate, 1);
  next.endDate = addDays(next.startDate, 6);

  let timeDifference = current.startDate - new Date(semester.value.startDate);
  current.number = Math.floor(timeDifference / (7 * 24 * 60 * 60 * 1000)) + 1;
  current.isUpper = (current.number % 2 === 1) === semester.value.evenIsUpper;

  next.number = current.number + 1;
  next.isUpper = !current.isUpper;

  return { current, next }
});

function addDays(date, days) {
  var result = new Date(date);
  result.setDate(result.getDate() + days);
  return result;
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

body {
  padding: 5px;
  background-color: rgb(15, 15, 15);
}

.text {
  color: white;
  align-self: center;
}
</style>
