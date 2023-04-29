<template>
  <div :class="['v-input', { _error: errors.length }]" v-auto-animate>
    <div v-if="!disabled" class="v-input__label-container">
      <label class="v-input__label">{{ label }}</label>
      <span v-if="required" class="_required">*</span>
      <v-tooltip
        v-if="tooltip"
        class="v-input__tooltip"
        :content="tooltip"
        placement="top"
      />
    </div>
    <div class="v-input__input-container" v-auto-animate>
      <div v-if="file" class="v-input__file">
        <v-icon :name="`icon_${file.type}`" width="30" height="40" />
        <div class="v-input__file-content">
          <a class="v-input__file-label" :href="file.url" target="_blank">
            {{ file.label }}
          </a>
          <p class="v-input__file-size" v-if="file.size">
            {{ formatSize(file.size) }}
          </p>
        </div>
        <v-button
          v-if="!disabled && asyncDeleteMethod"
          class="v-input__file-delete"
          @click="deleteFile"
          mode="icon"
        >
          <v-icon name="icon_close" width="24" />
        </v-button>
      </div>
      <el-upload
        v-else
        ref="input"
        class="v-input__input"
        :class="{ _disabled: disabled }"
        drag
        :disabled="disabled || progress_init"
        action="#"
        :on-change="uploadFile"
        :auto-upload="false"
        :show-file-list="false"
      >
        <div class="v-input__input-upload-text">
          <template v-if="progress_init">
            <p class="v-input__progress">Идет загрузка {{ progress }}%</p>
            <span>Пожалуйста, не обновляйте страницу</span>
          </template>
          <template v-else>
            <p class="_desktop">Перетащите файл или <em>загрузите</em></p>
            <p class="_mobile"><em>Загрузите</em></p>
            <span>
              Поддерживаемые форматы: {{ formats.join(" ").toUpperCase() }}
            </span>
          </template>
          <div
            class="v-input__progress-bar"
            :style="{ width: `${progress}%` }"
          />
        </div>
      </el-upload>
    </div>
    <div class="v-input__errors-container" v-if="errors.length">
      <span v-for="(error, i) in errors" :key="error + i">{{ error }}</span>
    </div>
    <slot />
  </div>
</template>

<script>
import notifications from "~/mixins/notifications";
import VTooltip from "~/components/ui-kit/v-tooltip";
import VButton from "~/components/ui-kit/v-button";
import VIcon from "~/components/icon/v-icon";

export default {
  name: "v-upload",
  mixins: [notifications],
  components: { VIcon, VButton, VTooltip },

  props: {
    label: {
      type: String,
      default: "",
    },
    required: {
      type: Boolean,
      default: false,
    },
    fileName: {
      type: String,
      required: true,
    },
    file: {
      type: Object | null,
    },
    asyncParams: {
      type: Object | null,
      default: null,
    },
    asyncUploadMethod: {
      type: String,
      required: true,
    },
    asyncDeleteMethod: {
      type: String,
    },
    formats: {
      type: Array,
      default: () => [],
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
    progress: 0,
    progress_init: false,
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
    async uploadFile(file) {
      try {
        const id = this.asyncParams ? this.asyncParams.id : undefined;
        const params = new FormData();
        params.append(this.fileName, file.raw);
        this.progress_init = true;

        const config = {
          onUploadProgress: ({ loaded, total }) => {
            this.progress = Math.round((loaded * 100) / total);
          },
        };

        if (id) {
          await this.$store.dispatch(this.asyncUploadMethod, {
            id,
            params,
            config,
          });
        } else {
          await this.$store.dispatch(this.asyncUploadMethod, {
            params,
            config,
          });
        }
      } catch (e) {
        this.renderAsyncErrors(e);
      } finally {
        this.progress_init = false;
        this.progress = 0;
      }
    },

    async deleteFile() {
      try {
        const { id } = this.asyncParams;
        if (id) {
          await this.$store.dispatch(this.asyncDeleteMethod, { id });
        } else {
          await this.$store.dispatch(this.asyncDeleteMethod);
        }
      } catch (e) {
        this.renderAsyncErrors(e);
      }
    },

    formatSize(length) {
      let size = length;
      let i = 0;
      const type = ["б", "Кб", "Мб", "Гб", "Тб", "Пб"];

      while ((size / 1000) | 0 && i < type.length - 1) {
        size /= 1024;
        i++;
      }

      return size.toFixed(2) + " " + type[i];
    },
  },
};
</script>
