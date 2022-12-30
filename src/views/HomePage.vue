<template>
      <add-task v-if="showAddTask" @add-new-task="addNewTask" />
    <my-tasks :tasks="tasks" @delete-task="deleteTask" @mark-as-complete="markAsComplete"/>
</template>

<script>
import MyTasks from '../components/MyTasks.vue'
import AddTask from '../components/AddTask.vue'

export default {
    name:'HomePage',
    components:{
        MyTasks,
        AddTask,
    },
    props:{
        showAddTask:Boolean
    },
    data(){
        return {
         tasks:[]
        }
    },
async created(){
    this.tasks = await this.fetchTasks();
  },
methods:{
    async deleteTask(id){
    const res =  await fetch(`/api/tasks/${id}`,{
        method:'DELETE',
      });
      res.status == 200 ? (this.tasks=this.tasks.filter(item=>item.id != id)) : alert("Internal server error")
    
    },
    async markAsComplete(id){
      const getTask = await this.fetchTask(id);
      const updatedTask = {...getTask,completed: !getTask.completed}
      const res = await fetch(`api/tasks/${id}`,{
        method:'PUT',
        headers:{
          'Content-Type':'application/json'
        },
        body:JSON.stringify(updatedTask)
      })
      const data = await res.json();
      this.tasks.map(item=>{
        if(item.id == id){
          item.completed = data.completed;
        }
      })
    },
    async addNewTask(task){
      const res = await fetch('/api/tasks',{
        method:'POST',
        headers:{
          'Content-type':'application/json',
        },
        body:JSON.stringify(task)
      });
     const data =  await res.json();
      this.tasks.push(data);
    },
    async fetchTasks(){
      const res = await fetch('/api/tasks')
      const data = await res.json();
      return data;
    },
    async fetchTask(id){
      const res = await fetch(`/api/tasks/${id}`)
      const data = await res.json();
      return data;
    }
  }
}
</script>