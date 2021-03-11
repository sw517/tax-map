<template>
  <div class="search-input">
    <input
      type="text"
      :value="value"
      @input="onInput"
      class="search-input__input"
      :placeholder="placeholder"
    />
  </div>
</template>
<script>
export default {
  name: 'SearchInput',
  props: {
    value: {
      type: String,
      default: '',
    },
    placeholder: {
      type: String,
      default: 'Search',
    },
  },
  methods: {
    onInput(e) {
      this.$emit('input', e.target.value)
    },
  },
}
</script>
<style lang="scss"></style>
