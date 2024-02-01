<template>
    <div class="week-container">
        <div class="week-container" style="margin-bottom: 20px;">
            <span class="week-name text">Неделя {{ startDateName }} – {{ endDateName }} </span>
            <span :style="{color:weekTypeColor}" class="text">{{ weekType }}</span>
        </div>

        <div v-if="notEmpty" class="days">
            <DaySchedule v-for="(day, index) in days" :dayInfo="day" :key="index" />
        </div>
        <span v-else class="text alert">
            Нет занятий
        </span>
    </div>
</template>

<script setup>
import DaySchedule from './DaySchedule.vue';
import { computed, defineProps } from 'vue'

const props = defineProps({
    week: Object,
    subjects: { type: Array, default() { return [] } },
    daysOff: { type: Array, default() { return [] } }
});

props.week.startDate.setHours(0, 0, 0, 0);
props.week.endDate.setHours(0, 0, 0, 0);

const days = computed(() => {
    let days = [];
    for (let i = 0; i < 7; i++) {
        let day = new Date(props.week.startDate);
        day.setDate(day.getDate() + i);

        let skip = props.daysOff.some(x =>
            x.toLocaleDateString("ru-Ru", { month: '2-digit', day: '2-digit', year: '2-digit' })
            === day.toLocaleDateString("ru-Ru", { month: '2-digit', day: '2-digit', year: '2-digit' }));

        if (skip) continue;

        let dayInfo = {};

        dayInfo.subjects = props.subjects.filter((x) =>
            x.isUpperWeek == props.week.isUpper
            && x.startWeek <= props.week.number
            && x.endWeek >= props.week.number
            && x.dayOfWeek == day.getDay());

        dayInfo.date = day;
        days.push(dayInfo);
        if (day.getDate() == props.week.endDate.getDate()) break;
    }
    return days;
});

const notEmpty = computed(() => {
    for (var day of days.value)
        if (day.subjects.length > 0) return true;
    return false;
})

const weekTypeColor = computed(() => props.week.isUpper ? "#73d97d" : "#ffe18c")
const weekType = computed(() => props.week.isUpper ? "верхняя" : "нижняя");
const startDateName = computed(() => props.week.startDate.toLocaleDateString("ru-Ru", { month: '2-digit', day: '2-digit' }));
const endDateName = computed(() => props.week.endDate.toLocaleDateString("ru-Ru", { month: '2-digit', day: '2-digit' }));
</script>

<style scoped>
.days {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.days>* {
    margin: 0px 10px 20px 10px;
}

.alert{
    font-weight: bolder;
    font-size: 20px;
    color: #cf3c6d;
}

.week-name {
    font-weight: bolder;
    font-size: 20px;
    text-transform: capitalize;
}

.week-container {
    display: flex;
    flex-flow: column;
    flex-wrap: wrap;
}
</style>