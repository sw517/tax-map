<template>
  <InfoPopup :can-close="true" class="tax-info-popup" @close="onClose">
    <div class="tax-info-popup__title">{{ title }}</div>
    <ul class="tax-info-popup__bullets" v-if="bullets && bullets.length">
      <li v-for="(bullet, index) in bullets" :key="`${index}-${bullet}`">
        {{ bullet }}
      </li>
    </ul>
    <a :href="link.href">{{ link.text }}</a>
  </InfoPopup>
</template>

<script>
import InfoPopup from '@/components/info-popups/InfoPopup'

export default {
  name: 'TaxInfoPopup',
  components: { InfoPopup },
  props: {
    title: {
      type: String,
      required: true,
    },
    bullets: {
      type: Array,
      default: () => [],
    },
    link: {
      type: Object,
      required: true,
      validator(link) {
        return link && 'href' in link && 'text' in link
      },
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
.tax-info-popup {
  &__title {
    font-weight: 700;
    margin-bottom: 16px;
  }

  &__bullets {
    padding-left: 20px;
  }
}
</style>
