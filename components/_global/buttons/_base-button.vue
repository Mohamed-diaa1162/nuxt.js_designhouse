<template>
  <button
    class="btn text-truncate font-16 fw-500"
    :class="{
      [`btn-${type}`]: true,
      ['btn-block']: block,
      [`btn-${size}`]: true,
    }"
    :type="nativeType"
    :disabled="loading"
  >
    <span v-if="loading === true">
      <i class="fas fa-spinner fa-spin"></i>
    </span>
    <slot v-else></slot>
  </button>
</template>

<script>
export default {
  name: 'base-button',
  props: {
    type: {
      type: String,
      default: 'primary',
    },
    nativeType: {
      type: String,
      default: 'submit',
    },
    loading: {
      type: Boolean,
      default: false,
    },
    block: {
      type: Boolean,
      default: false,
    },
    size: {
      type: String,
      default: 'md',
    },
  },
}
</script>
