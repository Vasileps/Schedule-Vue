<template>
    <div v-show="notEmpty" class="panel">
        <div class="day-name text">
            {{ dayName }}
        </div>
        <div v-if=isToday class="text" style="color: lightgreen; font-size: small;">
            Сегодня
        </div>
        <div v-else-if=isTomorrow class="text" style="color: lightyellow; font-size: small;">
            Завтра
        </div>
        <div v-else style="font-size: small;">ㅤ</div>
        <SubjectLabel v-for="subject in dayInfo.subjects" :subjectData="subject" :key="subject" style="margin-top: 10px;" />
    </div>
</template>

<script setup>
import { computed, defineProps, toRef } from 'vue'
import SubjectLabel from "./SubjectLabel.vue";

const props = defineProps({ dayInfo: { type: Object, required: true } });
const dayInfo = toRef(props, "dayInfo");

const dayName = computed(() => dayInfo.value.date.toLocaleDateString("ru-Ru", { weekday: 'long' }));
const isToday = computed(() => {
    let today = new Date();
    return dayInfo.value.date.setHours(0, 0, 0, 0) === today.setHours(0, 0, 0, 0);
})
const isTomorrow = computed(() => {
    let tomorrow = new Date();
    tomorrow.setDate(tomorrow.getDate() + 1);
    return dayInfo.value.date.setHours(0, 0, 0, 0) === tomorrow.setHours(0, 0, 0, 0);
})
const notEmpty = computed(() => dayInfo.value.subjects.length > 0);
</script>

<style scoped>
.panel {
    width: 300px;
    border-radius: 10px;
    border: 2px solid white;
    padding: 5px;
    background-color: rgb(75, 75, 75);
    display: flex;
    flex-flow: column;
    vertical-align: top;
}

.day-name {
    font-size: x-large;
    text-transform: capitalize;
    font-weight: bold;
    margin: 10px auto 0 auto;
}
</style>