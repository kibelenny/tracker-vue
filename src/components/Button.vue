<template>
  <button
    @click="$emit('btn-click')"
    class="btn"
    :style="{ background: color }"
  >
    {{ btnName }}
  </button>
</template>

<script>
export default {
  name: "Button",
  props: {
    btnName: String,
    color: String,
  },
};
</script>
