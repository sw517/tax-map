<template>
  <div class="social-share">
    <span>Share: </span>
    <a
      v-for="link in socialLinks"
      :key="link.name"
      :href="link.href"
      class="social-share__link"
    >
      <component :is="link.icon" class="social-share__icon" />
    </a>
  </div>
</template>
<script>
import FacebookIcon from '@/assets/icons/facebook.svg?inline'

export default {
  name: 'SocialShare',
  components: {
    FacebookIcon,
  },
  data() {
    return {
      socialLinks: [
        {
          name: 'Facebook',
          href: '/',
          icon: FacebookIcon,
        },
        {
          name: 'Twitter',
          href: '/',
          icon: FacebookIcon,
        },
        {
          name: 'Instagram',
          href: '/',
          icon: FacebookIcon,
        },
        {
          name: 'Whatsapp',
          href: '/',
          icon: FacebookIcon,
        },
      ],
    }
  },
}
</script>
<style lang="scss">
.social-share {
  display: flex;
  align-items: center;

  &__link {
    margin-left: 10px;
  }

  &__icon {
    color: #000;
    width: 30px;
    height: 30px;
  }
}
</style>
