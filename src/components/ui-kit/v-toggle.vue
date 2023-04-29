<template>
  <div class="v-toggle">
    <el-radio-group v-model="valueSetter">
      <el-radio-button
        v-for="item in options"
        :key="item.value"
        :label="item.value"
      >
        {{ item.label }}
      </el-radio-button>
    </el-radio-group>
  </div>
</template>

<script>
export default {
  name: "v-toggle",
  props: {
    value: {
      required: true,
    },
    options: {
      type: Array,
      default: () => [],
    },
  },

  computed: {
    valueSetter: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit("input", value);
      },
    },
  },
};
</script>
