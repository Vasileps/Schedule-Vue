<template>
    <div class="label">
        <span class="subject-name"> {{ subjectData.name }} </span>
        <span> {{ subjectData.time }} | {{ subjectData.classroom }} | {{ subjectData.type }}</span>     
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

.subject-name{
    font-weight: bold;
}
</style>