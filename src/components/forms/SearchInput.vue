<template>
  <div class="search-input">
    <input type="text" :value="value" @input="onInput" placeholder="Search" />
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
  },
  methods: {
    onInput(e) {
      this.$emit('input', e.target.value)
    },
  },
}
</script>
<style lang="scss"></style>
