<template>
  <div class="container">
    <Header
      @toggle-form="toggleForm"
      :showForm="showForm"
      title="Task Tracker"
    />
    <transition name="slide">
      <Form v-if="showForm" @add-task="addTask" />
    </transition>
  </div>
  <div class="container">
    <Tasks
      @delete-task="deleteTask"
      @toggle-reminder="toggleReminder"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import Header from "./components/Header";
import Tasks from "./components/Tasks";
import Form from "./components/Form";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    Form,
  },

  data() {
    return {
      tasks: [],
      showForm: false,
    };
  },

  methods: {
    toggleForm() {
      this.showForm = !this.showForm;
    },

    async deleteTask(id) {
      if (confirm("Are you sure")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });
        const data = await res.json();
        this.tasks = this.tasks.filter((task) => task.id != id);
      }
    },

    async toggleReminder(id) {
      const tasktoUpdate = await this.fetchTask(id);
      const updatedTask = { ...tasktoUpdate, reminder: !tasktoUpdate.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        body: JSON.stringify(updatedTask),
        headers: {
          "Content-Type": "application/json",
        },
      });

      const data = await res.json();
      this.tasks = this.tasks.map((task) =>
        task.id == id ? { ...task, reminder: data.reminder } : task
      );
    },

    async addTask(newTask) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(newTask),
      });

      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },

    async fetchTasks() {
      const res = await fetch("api/tasks");
      const data = await res.json();

      return data;
    },

    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();

      return data;
    },
  },

  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  margin-bottom: 0;
  overflow: auto;
  /* min-height: 300px; */
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.container:last-child {
  margin: 20px auto;
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
/* Style by chatGPT} */
.slide-enter-active,
.slide-leave-active {
  transition: all 0.3s ease;
}
.slide-enter,
.slide-leave-to {
  transform: translateY(-20px);
  opacity: 0;
}
</style>
