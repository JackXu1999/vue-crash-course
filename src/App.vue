<template>
  <div class="container">
    <h3>{{time}}</h3>

    <Header @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask"/>
    
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
    
    <h4>Tasks Completed Today:</h4>

    <router-view :showAddTask="showAddTask"></router-view>
    
    <Footer />
  </div>
  
</template>


<script>
import Header from './components/Header'
import Footer from './components/Footer'


export default {
  name: 'App',
  components: {
   Header,
   Footer,
  },
  data() {
    return {
      showAddTask: true,
      time: "",
    }
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    getTime() {
      const today = new Date();
      const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
      const minute = today.getMinutes() < 10 ? '0' + today.getMinutes(): today.getMinutes();
      const time = today.getHours() + ":" + minute;
      const dateTime = date +' '+ time;
      this.time = dateTime;
    }
  },
  created() {
    setInterval(this.getTime, 1000);
  }
}
</script>

<style scoped>
h3{
  text-align: center;
  margin-bottom: 10px;
}
</style>



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
