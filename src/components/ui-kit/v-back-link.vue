<template>
  <router-link class="v-back-link" :to="route">
    <v-icon name="icon_arrow_left_alt" width="40" height="11" />
    <span>{{ label }}</span>
  </router-link>
</template>

<script>
import VIcon from "~/components/icon/v-icon";
export default {
  name: "v-back-link",
  components: { VIcon },
  props: {
    label: {
      type: String,
      default: "Назад",
    },
    route: {
      type: Object,
      require: true,
    },
  },
};
</script>
