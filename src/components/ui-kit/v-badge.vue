<template>
  <div :class="['v-badge', `v-badge--${status}`]">
    <span>{{ label }}</span>
  </div>
</template>

<script>
export default {
  name: "v-badge",

  props: {
    label: {
      type: String,
    },
    status: {
      type: String,
      default: "primary", // primary | system | success
    },
  },
};
</script>
