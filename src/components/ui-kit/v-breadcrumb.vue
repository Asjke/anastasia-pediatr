<template>
  <el-breadcrumb class="v-breadcrumb" separator-class="el-icon-arrow-right">
    <el-breadcrumb-item
      v-for="(item, i) in items"
      :key="i"
      :to="item.route ? { name: item.route } : undefined"
      >{{ item.label }}</el-breadcrumb-item
    >
  </el-breadcrumb>
</template>

<script>
export default {
  name: "v-breadcrumb",

  props: {
    items: {
      type: Array,
      default: () => [],
    },
  },
};
</script>
