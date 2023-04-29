<template>
  <div :class="['v-input', `v-input--${size}`, { _error: errors.length }]">
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
      <input
        ref="input"
        class="v-input__custom-input"
        type="tel"
        maxlength="18"
        :value="value"
        :placeholder="placeholder"
        :disabled="disabled"
        @input="inputPhone($event)"
        @paste="pastePhone($event)"
        @keydown="keyDownPhone($event)"
        @focus="$emit('focus')"
        @blur="temp($event)"
      />
    </div>
    <div class="v-input__errors-container" v-auto-animate>
      <span v-for="(error, i) in errors" :key="error + i">{{ error }}</span>
    </div>
    <slot />
  </div>
</template>

<script>
import VTooltip from "~/components/ui-kit/v-tooltip";

export default {
  name: "v-phone-input",
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

  data: () => ({
    is_check: false,
  }),

  mounted() {
    if (this.focus) {
      this.$refs.input.focus();
    }
  },

  watch: {
    value(v) {
      if (v && !this.is_check) {
        this.$emit("input", this.formatValue(v));
        this.is_check = true;
      }
    },
  },

  methods: {
    getInputNumbersValue(input) {
      return input.value.replace(/\D/g, "");
    },

    formatValue(v) {
      const value = v.replace(/\D/g, "");
      let result = "";
      if (value.startsWith("7")) {
        result = "+7";
      }
      if (value.length > 1) {
        result += " (" + value.substring(1, 4);
      }
      if (value.length >= 5) {
        result += ") " + value.substring(4, 7);
      }
      if (value.length >= 8) {
        result += "-" + value.substring(7, 9);
      }
      if (value.length >= 10) {
        result += "-" + value.substring(9, 11);
      }
      return result;
    },

    temp(e) {
      this.$emit("input", this.formatValue(e.target.value));
      this.$emit("blur");
    },

    inputPhone(e) {
      const input = e.target;
      let inputNumbersValue = this.getInputNumbersValue(input);
      const selectionStart = input.selectionStart;
      let formattedInputValue = "";

      if (!inputNumbersValue) {
        this.$emit("input", "");
        input.value = "";
        return;
      }

      if (input.value.length !== selectionStart) {
        if (e.data && /\D/g.test(e.data)) {
          input.selectionStart = selectionStart - 1;
          input.selectionEnd = input.selectionStart;
          this.$emit("input", input.value);
        }
        return;
      }

      if (inputNumbersValue[0] === "9") {
        inputNumbersValue = "7" + inputNumbersValue;
      }
      const firstSymbols = inputNumbersValue[0] === "8" ? "+7" : "+7";
      formattedInputValue = firstSymbols + " ";
      if (inputNumbersValue.length > 1) {
        formattedInputValue += "(" + inputNumbersValue.substring(1, 4);
      }
      if (inputNumbersValue.length >= 5) {
        formattedInputValue += ") " + inputNumbersValue.substring(4, 7);
      }
      if (inputNumbersValue.length >= 8) {
        formattedInputValue += "-" + inputNumbersValue.substring(7, 9);
      }
      if (inputNumbersValue.length >= 10) {
        formattedInputValue += "-" + inputNumbersValue.substring(9, 11);
      }

      this.$emit("input", formattedInputValue);
      input.value = formattedInputValue;
    },

    keyDownPhone(e) {
      const inputValue = e.target.value.replace(/\D/g, "");
      if (e.keyCode === 8 && inputValue.length === 1) {
        this.$emit("input", "");
      }
    },

    pastePhone(e) {
      const input = e.target;
      const inputNumbersValue = this.getInputNumbersValue(input);
      const pasted = e.clipboardData || window.clipboardData;
      if (pasted) {
        const pastedText = pasted.getData("Text");
        if (/\D/g.test(pastedText)) {
          this.$emit("input", inputNumbersValue);
        }
      }
    },
  },
};
</script>
