<template>
  <el-tooltip
    class="v-tooltip"
    :content="content"
    :placement="placement"
    effect="light"
  >
    <v-icon name="icon_tooltip" width="16" />
  </el-tooltip>
</template>

<script>
import VIcon from "@/components/icon/v-icon";
export default {
  name: "v-tooltip",
  components: { VIcon },

  props: {
    content: {
      type: String,
      default: "",
    },
    placement: {
      type: String,
      default: "top",
    },
  },
};
</script>

<style scoped></style>
