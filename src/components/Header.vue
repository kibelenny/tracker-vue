<template>
  <header>
    <h1>
      {{ title }}
    </h1>
    <Button
      @btn-click="$emit('toggle-form')"
      :btnName="showForm ? 'Close' : 'Add Task'"
      :color="showForm ? 'red' : 'green'"
    />
  </header>
</template>

<script>
import Button from "./Button";

export default {
  name: "Header",
  props: {
    title: String,
    showForm: Boolean,
  },
  components: {
    Button,
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
