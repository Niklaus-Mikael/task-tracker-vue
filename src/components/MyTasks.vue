<template>
<div v-for="task in tasks" :key="task.id">
    <my-task :task="task" @delete-task="$emit('delete-task',task.id)" @mark-as-complete="$emit('mark-as-complete',task.id)"/>
</div>
</template>

<script>
import MyTask from './MyTask.vue';
export default {
    name:'my-tasks',
    props:{
        tasks:Array,
    },
    components:{
        MyTask,
    },
    emits:['delete-task','mark-as-complete']
}   
</script>
