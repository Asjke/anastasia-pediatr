<template>
  <div class="v-switcher">
    <el-switch
      v-model="valueSetter"
      :active-text="label"
      :width="44"
      @focus="$emit('focus')"
    />
  </div>
</template>

<script>
export default {
  name: "v-switcher",
  props: {
    value: {
      required: true,
    },
    label: {
      type: String,
      default: "",
    },
    required: {
      type: Boolean,
      default: false,
    },
    errors: {
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
