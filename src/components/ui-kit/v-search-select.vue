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
      <el-select
        class="v-input__input"
        :class="{ _disabled: disabled }"
        filterable
        remote
        clearable
        :remote-method="remoteMethod"
        v-model="valueSetter"
        :placeholder="placeholder"
        :disabled="disabled"
        loading-text="Идет поиск..."
        :no-data-text="emptyText"
        @focus="$emit('focus')"
      >
        <el-option
          v-for="item in options"
          :key="item.id"
          :label="item.value"
          :value="item"
        >
        </el-option>
      </el-select>
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
  name: "v-search-select",
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
    asyncParams: {
      type: Object | null,
      default: null,
    },
    asyncAction: {
      type: String,
      required: true,
    },
    asyncGetter: {
      type: String,
      required: true,
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
    emptyText: {
      type: String,
      default: "Нет данных",
    },
    errors: {
      type: Array,
      default: () => [],
    },
  },

  data: () => ({
    loading: false,
    options: [],
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

  watch: {
    value() {
      if (this.value && this.value.value && !this.options.length) {
        this.options.push(this.value);
      }
    },
  },

  methods: {
    async remoteMethod(query) {
      if (query !== "") {
        this.loading = true;
        if (this.asyncParams) {
          const data = this.asyncParams;
          await this.$store.dispatch(this.asyncAction, { query, data });
        } else {
          await this.$store.dispatch(this.asyncAction, { query });
        }
        if (this.$store.getters[this.asyncGetter] !== undefined) {
          this.options = this.$store.getters[this.asyncGetter];
        } else {
          this.options = [];
        }
        this.loading = false;
      } else {
        this.options = [];
      }
    },
  },
};
</script>
