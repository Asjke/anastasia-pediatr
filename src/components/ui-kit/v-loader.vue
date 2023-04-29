<template>
  <div class="v-loader">
    <div class="v-loader__element">
      <div v-for="div in 8" :key="div" />
    </div>
  </div>
</template>

<script>
export default {
  name: "v-loader",
};
</script>
