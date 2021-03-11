<template>
  <div class="info-popup">
    <slot />
    <button v-if="canClose" @click="onClose" class="info-popup__close">
      x
    </button>
  </div>
</template>
<script>
export default {
  name: 'InfoPopup',
  props: {
    canClose: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    onClose() {
      this.$emit('close')
    },
  },
}
</script>
<style lang="scss" scoped>
.info-popup {
  position: relative;
  box-shadow: 0 0 5px 2px rgba(0, 0, 0, 0.2);
  padding: 20px;

  &__close {
    appearance: none;
    -moz-appearance: none;
    -webkit-appearance: none;
    border: 0;
    background: none;
    padding: 10px;
    position: absolute;
    top: 0;
    right: 0;
    cursor: pointer;
    font-size: 18px;
  }
}
</style>
