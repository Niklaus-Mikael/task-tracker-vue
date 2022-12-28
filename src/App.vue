<template>
  <div class="container">
    <TaskHeader @toggle-add-task="toggleAddTask"  :showAddTask="showAddTask"/>
    <div v-if="showAddTask">
      <add-task @add-new-task="addNewTask" />
    </div>
    <my-tasks :tasks="tasks" @delete-task="deleteTask" @mark-as-complete="markAsComplete"/>
  </div>
</template>

<script>
import TaskHeader from './components/TaskHeader.vue'
import MyTasks from './components/MyTasks.vue'
import AddTask from './components/AddTask.vue'
export default {
  name: 'App',
  components:{
    TaskHeader,
    MyTasks,
    AddTask,
  },
  data(){
    return {
      tasks:[],
      showAddTask:false
    }
  },
   async created(){
    this.tasks = await this.fetchTasks();
  },
  methods:{
    async deleteTask(id){
      await fetch(`/api/tasks/${id}`,{
        method:'DELETE',
      });
    this.tasks=this.tasks.filter(item=>item.id != id);
    },
    markAsComplete(id){
      this.tasks.map(item=>{
        if(item.id == id){
          item.completed = !item.completed;
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
    toggleAddTask(){
        this.showAddTask = !this.showAddTask;
    },
    async fetchTasks(){
      const res = await fetch('/api/tasks')
      const data = await res.json();
      return data;
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
  cursor: pointer;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
