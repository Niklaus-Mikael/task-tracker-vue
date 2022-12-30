<template>
    <header>
        <h1>{{title}}</h1>
        <my-button v-show="toggleAddTask" @btn-click="$emit('toggle-add-task')" :label="showAddTask ?   'Close' : 'Add Task'" :color="showAddTask ? 'red' : 'green'"/>
    </header>
</template>

<script>
import MyButton from './MyButton.vue';
export default {
    name:'TaskHeader',
    props:{
        title:{
            type:String,
            default:'Task Tracker'
        },
        showAddTask:Boolean

    },
    computed:{
        toggleAddTask(){
            if(this.$route.path === '/'){
                return true;
            } else {
                return false;
            }
        }
    },
    components:{
        MyButton,
    }
}
</script>

<style scoped>
header{
display: flex;
justify-content: space-between;
align-items: center;
}
</style>