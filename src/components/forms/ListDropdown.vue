<template>
  <div class="list-dropdown">
    <div class="list-dropdown__inner">
      <button
        v-for="option in items"
        :key="option.region"
        class="list-dropdown__option"
        @click="$emit('click', option)"
      >
        {{ option.region }}
      </button>
    </div>
  </div>
</template>
<script>
export default {
  name: 'ListDropdown',
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
}
</script>
<style lang="scss">
.list-dropdown {
  max-height: 150px;
  overflow: auto;
  box-shadow: 0 0 5px 2px rgba(0, 0, 0, 0.2);
  background-color: #fff;

  &__option {
    appearance: none;
    -moz-appearance: none;
    -webkit-appearance: none;
    border: none;
    background: none;
    cursor: pointer;
    display: block;
    padding: 10px;
    width: 100%;
    text-align: left;

    &:hover {
      background-color: var(--theme-tertiary, #99b7ff);
    }
  }
}
</style>
