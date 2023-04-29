<template>
  <div class="v-file">
    <div class="v-file__file">
      <v-icon :name="`icon_${format}`" width="30" height="40" />
      <div class="v-file__file-content">
        <span class="v-file__file-label">
          {{ label }}
        </span>
        <p class="v-file__file-size" v-if="size">
          {{ formatSize(size) }}
        </p>
      </div>
    </div>
    <div class="v-file__btn-group">
      <v-link
        class="v-file__btn"
        mode="blue-transparent"
        size="small"
        weight="medium"
        :url="url"
        :download="label"
        target="_blank"
        icon-left
      >
        <v-icon name="icon_plus" width="16" />
        {{ button_label }}
      </v-link>
    </div>
  </div>
</template>

<script>
import VIcon from "~/components/icon/v-icon";
import VButton from "~/components/ui-kit/v-button";
import VLink from "~/components/ui-kit/v-link";

export default {
  name: "v-file",
  components: { VLink, VButton, VIcon },
  props: {
    label: {
      type: String,
      default: "",
    },
    format: {
      type: String,
      default: "pdf",
    },
    size: {
      type: Number,
      default: null,
    },
    url: {
      type: String,
      default: "",
    },
    button_label: {
      type: String,
      default: "Скачать",
    },
  },

  methods: {
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
