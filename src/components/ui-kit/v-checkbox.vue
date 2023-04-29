<template>
  <div :class="['v-checkbox', `v-checkbox--${size}`]">
    <div class="v-checkbox__container">
      <el-checkbox
        class="v-checkbox__input"
        v-model="valueSetter"
        :disabled="disabled"
        @focus="$emit('focus')"
        @change="$emit('change', $event)"
      />
      <span class="v-checkbox__label" v-html="label" />
    </div>
    <div class="v-checkbox__errors-container" v-auto-animate>
      <span v-for="(error, i) in errors" :key="error + i">{{ error }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "v-checkbox",
  props: {
    value: {
      required: true,
    },
    label: {
      type: String,
      default: "",
    },
    size: {
      type: String,
      default: "normal", // normal | small
    },
    required: {
      type: Boolean,
      default: false,
    },
    disabled: {
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
