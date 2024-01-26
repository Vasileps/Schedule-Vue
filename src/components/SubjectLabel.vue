<template>
    <div class="label">
        <span> {{ subjectData.time }} {{ subjectData.classroom }} {{ subjectData.type }}</span>
        <span style="font-style: italic;"> {{ subjectData.name }} </span>
    </div>
</template>

<script>
export default {
    props: {
        subjectData: {type: Object, required: true}
    }
}
</script>

<style scoped>
.label {
    display: flex;
    flex-flow: column;
    flex-wrap: wrap;
}

.label>span {
    color: white;
}
</style>