<template>
  <div :key="tasks.id" v-for="task in tasks">
    <Task
      @delete-task="$emit('delete-task', task.id)"
      @dblclick="$emit('toggle-reminder', task.id)"
      :task="task"
    />
  </div>
</template>

<script>
import Task from "./Task";
export default {
  name: "Tasks",
  components: {
    Task,
  },
  props: {
    tasks: Object,
  },
  emits: ["delete-task", "toggle-reminder"],
};
</script>
