<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'
export default {
  name: 'Home',
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    async addTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      })
      const data = await res.json()
      if (data.priority === 'top') {
        this.tasks = [data, ...this.tasks]
      } else if (data.priority === 'second') {
        let newTask = [];
        let i = 0;
        while (i < this.tasks.length && this.tasks[i].priority === 'top') {
          newTask.push(this.tasks[i]);
          i++;
        }
        newTask.push(data);
        while (i < this.tasks.length) {
          newTask.push(this.tasks[i]);
          i++;
        }
        this.tasks = newTask;
      } else {
        this.tasks = [...this.tasks, data]
      }

      // this.tasks = [...this.tasks, data]
    },
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        })
        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert('Error deleting task')
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id)
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder }
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask),
      })
      const data = await res.json()
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      )
    },
    async fetchTasks() {
      const res = await fetch('api/tasks')
      const data = await res.json()
      const newData = [];

      for (let i = 0; i < data.length; i++) {
        if (data[i].priority === 'top') newData.push(data[i]);
      }
      for (let i = 0; i < data.length; i++) {
        if (data[i].priority === 'second') newData.push(data[i]);
      }
      for (let i = 0; i < data.length; i++) {
        if (data[i].priority === 'third') newData.push(data[i]);
      }

      return newData;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      return data
    },
  },
  async created() {
    this.tasks = await this.fetchTasks()
  },
}
</script>