<template>

  <h1>Task Manager</h1>

  <div class="row">
     
    <div class="col s12">
      <input class="card-panel teal lighten-2"
        v-model="newTask"
        @keyup.enter="addTask"
        autofocus
        autocomplete="off"
        placeholder="Введите задачу"
        value=""
      >
    </div>

    <TaskList :tasks="tasks"
    :removeTask="removeTask"
    :enterEditor="enterEditor"
    :exitEditor="exitEditor"
    :editorTask="editorTask"
    :sortingTask="sortingTask"
    />

  </div>

</template>



<script>
import TaskList from '../components/TaskList.vue'

export default {
  name: 'TaskManager',
  data() {
    return{
      tasks: [
        {
        id: "1",
        text: "Task 1",
        completed: false
        },
         {
        id: "2",
        text: "Task 2",
        completed: true
        },
        {
        id: "3",
        text: "Task 3",
        completed: false
        }
      ],
      newTask: "",
      inputId: null,
      beforeEditCache: null
    }
  },
  methods: {
    sortingTask: function () {
      const sortByCompleted = function (d1, d2) {return (d1.completed > d2.completed) ? 1 : -1;};
      return (this.tasks.slice().sort(sortByCompleted))
    },
    removeCompleted: function() {
     this.tasks = this.tasks.filter((task) => !task.completed)
    },
    editorTask: function(task) {
      this.beforeEditCache = task.text;
      this.inputId = task.id;
    },
    enterEditor: function(task) {
      if (!this.inputId) {
      return;
      }
      this.inputId = null;
      if (!task.text) {
      this.removeTask(task);
      }
    },
    exitEditor: function(task) {
      this.inputId = null;
      task.text = this.beforeEditCache;
    },
    removeTask: function(task) {
      this.tasks.splice(this.tasks.indexOf(task), 1);
    },
    createNewId() {
      let maxId = Math.max(...this.tasks.map(i => i.id));
      let newId = (maxId > 0) ? maxId+1 : 1
      return(newId);
    },
    addTask: function (){
      let value = this.newTask.trim();
      let date = this.newDate;
        if (!this.newTask) {
          return;
        }
      this.tasks.push({
        id: this.createNewId(),
        text: value,
        data: date,
        completed: false
      });
      this.newTask = "";
    }
  },
  components: {
    TaskList
  }
}
</script>

<style>
body {
  font: 14px "Helvetica Neue", Helvetica, Arial, sans-serif;
  line-height: 1.4em;
  background: #f8eba1;
  color: #4d4d4d;
  min-width: 230px;
  max-width: 90%;
  margin: 0 auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 300;
}
.todo{
  font-size: 18px;
  color: black
}
.active {
  text-decoration: line-through;
  color:#666666
}
.task{
  height: 50px;
  display: flex;
  align-items: center;
}
.del{
  margin-left: auto;
}
</style>
