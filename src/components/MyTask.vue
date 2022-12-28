<template>
    <div @dblclick="$emit('mark-as-complete',task.id)" :class="[task.completed ? 'reminder':'','task']">
        <div class="heading">
            <h3>{{ task.title }}</h3>
           <h3>
            <i @click="onDelete()" class="fas fa-times"></i>
           </h3> 
        </div>
        <p>{{ task.day }}</p>
    </div>
</template>

<script>
export default {
    name:'my-task',
    props:{
        task:Object,
    },
    methods:{
        onDelete(id){
            this.$emit('delete-task',id);
        }
    },
    emits:['delete-task','mark-as-complete']
}
</script>

<style scoped>
.fas{
    color:red;
    margin-left:auto;
}
.task {
    background:#f4f4f4;
    margin:5px;
    padding: 10px 20px;
    cursor: pointer;
}
.task > .heading{
    display: flex;
    justify-content: space-between;
}
.task.reminder {
    border-left: 5px solid green;
}
</style>
