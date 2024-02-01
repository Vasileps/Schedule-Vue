<template>
    <div v-show="notEmpty" class="panel">
        <div class="text day-name">
            {{ dayName }}
        </div>
        <div class="text day-text" style=" margin-bottom: 5px;">
            {{ date }}
            <span v-if="hasComment" class="text day-text" :style="{ color: dayCommentColor }">
                {{ dayComment }}
            </span>
        </div>
        <SubjectLabel v-for="(subject, index) in dayInfo.subjects" :subjectData="subject" :key="index"
            style="padding: 5px 0px 5px 0px" />
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
    let dayCopy = new Date(dayInfo.value.date.valueOf());
    today.setHours(0, 0, 0, 0);
    dayCopy.setHours(0, 0, 0, 0);

    return today.getTime() === dayCopy.getTime();
})
const isTomorrow = computed(() => {
    let tomorrow = new Date();
    let dayCopy = new Date(dayInfo.value.date.valueOf());
    tomorrow.setDate(tomorrow.getDate() + 1);
    tomorrow.setHours(0, 0, 0, 0);
    dayCopy.setHours(0, 0, 0, 0);

    return tomorrow.getTime() === dayCopy.getTime();
})
const hasComment = computed(() => isToday.value || isTomorrow.value)
const dayComment = computed(() => {
    if (isToday.value) return "Сегодня";
    else if (isTomorrow.value) return "Завтра";
    else return "ㅤ";
})
const dayCommentColor = computed(() => {
    if (isToday.value) return "#00fff7";
    else if (isTomorrow.value) return "#02bab4";
    else return "white";
})
const date = computed(() => dayInfo.value.date.toLocaleDateString("ru-Ru", { month: '2-digit', day: '2-digit' }));
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

.day-text {
    font-size: small;
}

.day-name {
    font-size: x-large;
    text-transform: capitalize;
    font-weight: bold;
}
</style>