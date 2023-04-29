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
    <div class="v-input__input-container">
      <!--      <el-input-->
      <!--        ref="input"-->
      <!--        class="v-input__input"-->
      <!--        :class="{ _disabled: disabled, _password: type === 'password' }"-->
      <!--        v-model="valueSetter"-->
      <!--        :type="accepted_type"-->
      <!--        :placeholder="placeholder"-->
      <!--        :disabled="disabled"-->
      <!--        :readonly="readonly"-->
      <!--        autocomplete="new-password"-->
      <!--        v-mask="mask"-->
      <!--        @focus="onFocus"-->
      <!--        @change="$emit('change')"-->
      <!--        @blur="$emit('blur')"-->
      <!--      />-->
      <input
        ref="input"
        class="v-input__custom-input"
        :class="{ _disabled: disabled, _password: type === 'password' }"
        v-model="valueSetter"
        :type="accepted_type"
        :placeholder="placeholder"
        :disabled="disabled"
        autocomplete="new-password"
        v-mask="mask"
        @focus="onFocus"
        @change="$emit('change')"
        @blur="$emit('blur')"
        @paste="handlePaste($event)"
      />
      <button
        v-if="type === 'password'"
        class="v-input__password-btn"
        type="button"
        @click="show_password = !show_password"
      >
        <v-icon
          :name="show_password ? 'icon_hide_password' : 'icon_show_password'"
          width="28"
        />
      </button>
    </div>
    <div class="v-input__errors-container" v-auto-animate>
      <span v-for="(error, i) in errors" :key="error + i">{{ error }}</span>
    </div>
    <slot />
  </div>
</template>

<script>
import VTooltip from "@/components/ui-kit/v-tooltip";
import VIcon from "~/components/icon/v-icon";

export default {
  name: "v-input",
  components: { VIcon, VTooltip },
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
    type: {
      type: String,
      default: "text", // text
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
    mask: {
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
    trim: {
      type: Boolean,
      default: false,
    },
  },

  data: () => ({
    show_password: false,
    readonly: true,
  }),

  computed: {
    valueSetter: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit("input", this.trim ? value.trim() : value);
      },
    },

    accepted_type() {
      if (this.type === "password" && this.show_password) {
        return "text";
      } else if (this.type === "password" && this.show_password === false) {
        return "password";
      } else {
        return this.type;
      }
    },
  },

  mounted() {
    if (this.focus) {
      this.$refs.input.focus();
    }
  },

  methods: {
    onFocus() {
      this.$emit("focus");
    },

    handlePaste(e) {
      // if (this.mask) {
      //   const value = e.clipboardData.getData("text/plain");
      //   this.$emit("input", this.trim ? value.trim() : value);
      // }
    },
  },
};
</script>
