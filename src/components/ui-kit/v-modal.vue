<template>
  <el-dialog
    :visible.sync="show"
    :custom-class="styles"
    top="10vh"
    center
    :destroy-on-close="destroy"
    :close-on-click-modal="false"
    @close="$emit('close')"
  >
    <div ref="modal">
      <slot />
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: "v-modal",

  data: () => ({
    show: false,
  }),

  props: {
    is_show: {
      type: Boolean,
      default: false,
    },
    is_error: {
      type: Boolean,
      default: false,
    },
    custom_class: {
      type: String,
      default: "",
    },
    destroy: {
      type: Boolean,
      default: false,
    },
  },

  watch: {
    is_show(value) {
      this.show = value;
    },
  },

  computed: {
    styles() {
      const style = `app-modal ${this.custom_class}`;
      if (this.is_error) {
        return style + " animate__animated animate__headShake";
      } else {
        return style;
      }
    },
  },
};
</script>
