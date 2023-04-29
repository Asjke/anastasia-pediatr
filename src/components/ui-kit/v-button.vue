<template>
  <button
    :class="[
      'v-button',
      `v-button--${mode}`,
      `v-button--${size}`,
      `v-button--${weight}`,
      { '_icon-right': iconRight, '_icon-left': iconLeft },
    ]"
    @click="$emit('click', $event)"
    :type="type"
    :disabled="disabled"
  >
    <slot />
  </button>
</template>

<script>
export default {
  name: "v-button",

  props: {
    mode: {
      type: String,
      default: "blue", // blue | blue-transparent | red | red-transparent | red-white | orange-transparent | grey | icon | link
    },
    size: {
      type: String,
      default: "normal", // small | normal
    },
    type: {
      type: String,
      default: "button", // native types
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
    weight: {
      type: String,
      default: "bold", // regular | medium | semibold | bold | extrabold | black
    },
  },
};
</script>
