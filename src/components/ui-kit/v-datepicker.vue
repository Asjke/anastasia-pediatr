<template>
  <div :class="['v-input', `v-input--${size}`, { _error: errors.length }]">
    <div class="v-input__label-container">
      <label class="v-input__label">
        {{ label }}
        <span v-if="required" class="_required">*</span>
      </label>
      <v-tooltip
        v-if="tooltip"
        class="v-input__tooltip"
        :content="tooltip"
        placement="top"
      />
    </div>
    <p v-if="prompt" class="v-input__prompt">{{ prompt }}</p>
    <div class="v-input__input-container">
      <el-date-picker
        ref="input"
        class="v-input__input"
        :class="{ _disabled: disabled }"
        v-model="valueSetter"
        :type="type"
        :format="format"
        :value-format="type === 'year' ? 'yyyy' : 'yyyy-MM-dd'"
        :placeholder="placeholder"
        :disabled="disabled"
        :picker-options="with_options ? picker_options : false"
        @focus="$emit('focus')"
        @blur="$emit('blur')"
      />
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
  name: "v-datepicker",
  components: { VTooltip },
  props: {
    value: {
      required: true,
    },
    size: {
      type: String,
      default: "normal", // normal | small
    },
    label: {
      type: String,
      default: "",
    },
    prompt: {
      type: String,
      default: "",
    },
    type: {
      type: String,
      default: "date", // year | date
    },
    format: {
      type: String,
      default: "dd.MM.yyyy",
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
    with_options: {
      type: Boolean,
      default: false,
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

  data: () => ({
    picker_options: {
      disabledDate(time) {
        return time.getTime() > Date.now();
      },
    },
  }),

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

  methods: {
    handleChange(e) {
      console.log(e);
    },
  },
};
</script>
