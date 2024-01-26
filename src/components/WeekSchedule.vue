<template>
    <div class="week">
        <span v-if="week.isUpper" class="week-text text">Верхняя неделя</span>
        <span v-else class="week-text text">Нижняя неделя</span>
        <span class="week-text text">
            {{ startDateName }} - {{ endDateName }}
        </span>
        <div class="days">
            <DaySchedule v-for="day in days" :dayInfo="day" :key="day" />
        </div>
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

.week-text {
    font-weight: bolder;
    font-size: 20px;
    padding-bottom: 10px;
    text-transform: capitalize;
    text-align: center;
}

.week {
    display: flex;
    flex-flow: column;
    flex-wrap: wrap;
}
</style>