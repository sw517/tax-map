<template>
  <div class="social-share">
    <span>Share: </span>
    <a
      v-for="link in socialLinks"
      :key="link.name"
      :href="link.href"
      :title="`Share to ${link.name}`"
      :class="[
        'social-share__link',
        `social-share__link--${kebabCase(link.name)}`,
      ]"
      target="_blank"
    >
      <component :is="link.icon" class="social-share__icon" />
    </a>
  </div>
</template>
<script>
import FacebookIcon from '@/assets/icons/facebook.svg?inline'
import TwitterIcon from '@/assets/icons/twitter.svg?inline'
import EmailIcon from '@/assets/icons/email.svg?inline'
import LinkedInIcon from '@/assets/icons/linkedin.svg?inline'

export default {
  name: 'SocialShare',
  props: {
    shareUrl: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      socialLinks: [
        {
          name: 'Facebook',
          href: `https://www.facebook.com/sharer/sharer.php?u=${this.shareUrl}`,
          icon: FacebookIcon,
        },
        {
          name: 'Twitter',
          href: `https://twitter.com/intent/tweet?url=${this.shareUrl}&text=`,

          icon: TwitterIcon,
        },
        {
          name: 'LinkedIn',
          href: `https://www.linkedin.com/shareArticle?mini=true&url=${this.shareUrl}`,

          icon: LinkedInIcon,
        },
        {
          name: 'Email',
          href: `mailto:info@example.com?&subject=&cc=&bcc=&body=${this.shareUrl}%0A`,

          icon: EmailIcon,
        },
      ],
    }
  },
  methods: {
    kebabCase(str) {
      return str.replace(/ /g, '-').toLowerCase()
    },
  },
}
</script>
<style lang="scss">
.social-share {
  display: flex;
  align-items: center;

  &__link {
    margin-left: 10px;

    &--facebook {
      color: #3c5998;
    }

    &--twitter {
      color: #1da1f1;
    }

    &--linkedin {
      color: #0b65c2;
    }

    &--email {
      color: var(--theme-tertiary);
    }
  }

  &__icon {
    width: 30px;
    height: 30px;
  }
}
</style>
