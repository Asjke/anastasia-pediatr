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
      <el-autocomplete
        ref="input"
        class="v-input__input"
        :class="{ _disabled: disabled }"
        v-model="valueSetter"
        :placeholder="placeholder"
        :disabled="disabled"
        :fetch-suggestions="querySearch"
        :trigger-on-focus="false"
        :debounce="600"
        @select="handleSelect"
        @focus="$emit('focus')"
        @blur="handleBlur"
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
  name: "v-autocomplete",
  components: { VTooltip },
  props: {
    value: {
      required: true,
    },
    size: {
      type: String,
      default: "normal", // normal | small
    },
    asyncParams: {
      type: Object | null,
      default: null,
    },
    asyncMethod: {
      type: String,
      required: true,
    },
    list: {
      type: String,
      required: true,
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
    index: {
      type: Number,
      default: null,
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

  methods: {
    async querySearch(queryString, cb) {
      const query = queryString;
      if (this.asyncParams) {
        const data = this.asyncParams;
        await this.$store.dispatch(this.asyncMethod, { query, data });
      } else {
        await this.$store.dispatch(this.asyncMethod, { query });
      }
      if (this.$store.getters[this.list] !== undefined) {
        const list = JSON.parse(JSON.stringify(this.$store.getters[this.list]));
        list.length ? cb(list) : cb([]);
      } else {
        cb([]);
      }
    },

    handleSelect(item) {
      if (this.index !== null) {
        this.$emit("handleSelect", item, this.index);
      } else {
        this.$emit("handleSelect", item);
      }
    },

    handleBlur() {
      setTimeout(() => {
        this.$emit("blur");
      }, 200);
    },
  },
};
</script>
