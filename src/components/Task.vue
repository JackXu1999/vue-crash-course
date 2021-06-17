<template>
    <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder' : '', 'task', task.priority, task.category]">
        <h3>
            {{task.text}}
            <!-- <i @click="$emit('complete-task', task.id)" class="fas fa-check"></i> -->
            <i @click="$emit('delete-task', task.id)" class="fas fa-times"></i>
        </h3>
        <p>{{task.date.substring(0, 10)}}</p>
    </div>

</template>

<script>
export default {
    name: 'Task',
    props: {
        task: Object,
    },
    
}
</script>

<style scoped>
.fas.fa-times {
  color: red;
}
.fas.fa-check {
  color: green;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.task.top {
  background-color: rgb(250, 205, 176);
} 
.task:hover {
  font-size: 115%;
} 
.task.second {
  background-color: rgb(171, 238, 171);
}
.task.third {
  background-color: lightcyan;
}
.task.study {
  border-right: 5px solid violet;
}
.task.work {
  border-right: 5px solid blue;
}
.task.leisure {
  border-right: 5px solid tomato;
}
.task.shopping {
  border-right: 5px solid orange;
}
</style>
