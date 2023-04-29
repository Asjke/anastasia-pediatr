<template>
  <div
    :class="['v-input', `v-input--${size}`, { _error: errors.length }]"
    v-auto-animate
  >
    <div class="v-input__label-container">
      <label class="v-input__label">{{ label }}</label>
      <span v-if="required" class="_required">*</span>
      <v-tooltip
        v-if="tooltip"
        class="v-input__tooltip"
        :content="tooltip"
        placement="top"
      />
    </div>
    <div class="v-input__input-container">
      <el-input
        ref="input"
        class="v-input__input"
        :class="{ _disabled: disabled }"
        v-model="valueSetter"
        type="textarea"
        :rows="rows"
        resize="none"
        :placeholder="placeholder"
        :maxlength="maxlength"
        :disabled="disabled"
        @focus="$emit('focus')"
      />
      <div class="v-input__textarea-max-length" v-if="maxlength">
        Символов: <strong>{{ value.length }}</strong> / 0 :
        {{ maxlength }}
      </div>
    </div>
    <div class="v-input__errors-container" v-auto-animate>
      <span v-for="(error, i) in errors" :key="error + i">{{ error }}</span>
    </div>
    <slot />
  </div>
</template>

<script>
import VTooltip from "@/components/ui-kit/v-tooltip";
export default {
  name: "v-textarea",
  components: { VTooltip },
  props: {
    value: {
      required: true,
    },
    size: {
      type: String,
      default: "normal", // normal | small
    },
    rows: {
      type: Number,
      default: 2,
    },
    label: {
      type: String,
      default: "",
    },
    maxlength: {
      type: String,
      default: "4000",
    },
    placeholder: {
      type: String,
      default: "",
    },
    required: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    tooltip: {
      type: String,
      default: "",
    },
    focus: {
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

  mounted() {
    if (this.focus) {
      this.$refs.input.focus();
    }
  },
};
</script>
