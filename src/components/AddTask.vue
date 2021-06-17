<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input type="text" v-model="text" name="text" placeholder="Add Task" />
    </div>
    
    <div class="form-control">
      <label>Day & Time</label>
      <datepicker class="datepicker" v-model="date" />
    </div>


    <div class="form-control form-control-select">
      <label>Priority</label>
      <select 
        type="text"
        v-model="priority"
        name="priority"
      >
        <option value="top" selected>Top</option>
        <option value="second">Second</option>
        <option value="third">Third</option>
      </select>
    </div>

    <div class="form-control form-control-select">
      <label>Category</label>
      <select
        type="text"
        v-model="category"
        name="category"
      >
        <option value="study" selected>Study</option>
        <option value="work">Work</option>
        <option value="leisure">Leisure</option>
        <option value="shopping">Shopping</option>
      </select>
    </div>



    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input type="checkbox" v-model="reminder" name="reminder" />
    </div>

    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script>
import Datepicker from 'vue3-datepicker'
import { ref } from 'vue'


export default {
  name: 'AddTask',
  components: {
    Datepicker,
  },
  data() {
      return {
          priority: 'top',
          category: 'study',
          text: '',
          reminder: false,
          date: ref(new Date()),
      }
  },
  methods: {
      onSubmit(e) {
        e.preventDefault();
        if (!this.text) {
            alert('Please add a task');
            return;
        }
        const newTask = {
            // id: Math.floor(Math.random() * 100000),
            priority: this.priority,
            category: this.category,
            date: this.date, 
            text: this.text, 
            reminder: this.reminder,
        }
        // console.log(newTask.date);
        this.$emit('add-task', newTask);

        this.text = '';
        this.reminder = false;
        this.priority = 'top';
        this.category = 'study';
        this.date = ref(new Date());
      }
  },
}
</script>

<style scoped>

.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

.form-control-select {
  display: flex;
  justify-content: space-between;
  text-align-last:center;
  height: 20px;
}


.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 10;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
.btn.btn-block{
  background-color: forestgreen;
}
</style>

<style>
.datepicker {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
</style>
