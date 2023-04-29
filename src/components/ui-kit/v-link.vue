<template>
  <a
    :class="[
      'v-button',
      `v-button--${mode}`,
      `v-button--${size}`,
      `v-button--${weight}`,
      { '_icon-right': iconRight, '_icon-left': iconLeft },
    ]"
    @click="$emit('click', $event)"
    :href="url"
    :target="target"
    :disabled="disabled"
    :download="download"
  >
    <slot />
  </a>
</template>

<script>
export default {
  name: "v-link",

  props: {
    mode: {
      type: String,
      default: "blue", // blue | blue-transparent | red | red-transparent | grey | icon
    },
    target: {
      type: String,
      default: "", // native types
    },
    size: {
      type: String,
      default: "normal", // small | normal
    },
    url: {
      type: String,
      default: "#",
    },
    iconRight: {
      type: Boolean,
      default: false,
    },
    iconLeft: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    download: {
      type: Boolean | String,
      default: false,
    },
    weight: {
      type: String,
      default: "bold", // regular | medium | semibold | bold | extrabold | black
    },
  },
};
</script>
