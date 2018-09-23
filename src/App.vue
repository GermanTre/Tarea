<template>
  <div>
    <h1>My ToDo App</h1>
    <form @submit.prevent="addTask">
      <input v-model="newTaskName" type="text">
      <input class="btn btn-primary" type="submit" value="Agregar Tarea">
    </form>

    <tasks-list
      :title="'Pendientes'"
      :task-list="tasksPending"
      @completar="toggleTasks"/>
    <hr>

    <tasks-list
      :title="'Completados'"
      :task-list="tasksComplete"
      @completar="toggleTasks"/>

      
  </div>
</template>

<script>

import TasksList from './components/TasksList'


if(JSON.parse(localStorage.getItem("tareas")) == null){
  var tareas = new Array()
  tareas.push({name: "", done : ""})
  localStorage.setItem('tareas', JSON.stringify(tareas))
}else{
  //localStorage.clear()
  var tareas = JSON.parse(localStorage.getItem("tareas"))
}

var storedTareas = JSON.parse(localStorage.getItem("tareas"));

export default {
  components: {
    TasksList
  },
  data () {
    return {
      newTaskName: '',
      tasks: storedTareas
      
    }
  },
  methods: {
    toggleTasks (task) {
      task.done = !task.done
      var a = true;
      tareas.forEach(function(element) {
        if(element.name == task.name && a){
          element.done = !element.done
          localStorage.setItem('tareas', JSON.stringify(tareas))
          var storedTareas = JSON.parse(localStorage.getItem("tareas"))
          a = false
        }
      });
    },
    addTask () {
      if (!this.newTaskName) return
      this.tasks.push({ name: this.newTaskName, done: false })
      tareas.push({name: this.newTaskName, done : false})
      this.newTaskName = ''
      localStorage.setItem('tareas', JSON.stringify(tareas))
      var storedTareas = JSON.parse(localStorage.getItem("tareas"));
      console.log(storedTareas)
    }
  },
  computed: {
    tasksPending () {
      return this.tasks.filter(task => !task.done && task.name != "")
    },
    tasksComplete () {
      return this.tasks.filter(task => task.done  && task.name != "")
    }
  }
}

</script>

<style>
body {
    background-color: coral;
}
</style>