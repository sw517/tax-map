<template>
  <EuropeSVG />
</template>

<script>
import EuropeSVG from '@/assets/maps/europe_map.svg?inline'

export default {
  name: 'EuropeMap',
  components: { EuropeSVG },
  props: {
    activeRegionId: {
      type: String,
      default: '',
    },
    regions: {
      type: Array,
      required: true,
    },
  },
  watch: {
    activeRegionId(id) {
      const previousActiveRegion = this.$el.querySelector('.active-region')
      if (previousActiveRegion) {
        previousActiveRegion.classList.remove('active-region')
      }

      if (id) {
        this.$el.querySelector(`#${id}`).classList.add('active-region')
      }
    },
  },
  mounted() {
    this.setUpRegionPaths()
  },
  methods: {
    /**
     * Add a listener for click and keydown events to a region
     * so it will emit the country code to parent.
     * @param { node } el The element to apply listeners.
     * @param { string } arg.countrycode The country code to emit.
     */
    addMapPathListener(el, { countryCode }) {
      el.addEventListener('click', () => this.emitClick(countryCode))
      el.addEventListener('keydown', e => {
        if (e.code === 'Space' || e.code === 'Enter') {
          e.preventDefault()
          this.emitClick(countryCode)
        }
      })
    },
    /**
     * Apply classes and attributes to element so we
     * can target with CSS and apply better UX.
     * @param { node } el The element to apply classes and attributes.
     * @param { string } arg.region The name of the region.
     */
    applyAttributesAndClasses(el, { region }) {
      el.classList.add('has-data')
      el.tabIndex = 0
      el.setAttribute('title', region)
      el.setAttribute(
        'aria-label',
        `${region}. Click to find out tax information for this region`
      )
      el.setAttribute('role', 'button')
    },
    emitClick(countryCode) {
      this.$emit('path-click', countryCode)
    },
    /**
     * Loop through the "regions" prop to apply attributes
     * and listeners to corresponding path elements.
     */
    setUpRegionPaths() {
      this.regions.forEach(region => {
        const regionElement = this.$el.getElementById(
          region.countryCode.toLowerCase()
        )
        if (regionElement) {
          this.applyAttributesAndClasses(regionElement, region)
          this.addMapPathListener(regionElement, region)
        }
      })
    },
  },
}
</script>

<style scoped lang="scss">
// Styles copied from SVG inline.
// These are stripped during compilation so are
// defined here as fallback.
// ===================================================
svg {
  color: #e0e0e0;
}

g,
path,
.nationGroup {
  fill: currentColor;
  stroke: white;
  stroke-width: 8;
}

/* Geographic Features */
#ocean {
  fill: none;
}
#lakes {
  opacity: 0.6;
  fill: #fff;
  stroke: none;
}

// Overide-styles for SVG map:
// ===================================================
.has-data {
  fill: #9b9b9b;
  color: #9b9b9b;
  cursor: pointer;
}
// Default state if id exists

// Hover state
.has-data:hover,
.has-data:focus {
  fill: var(--theme-tertiary, #99b7ff);
  color: var(--theme-tertiary, #99b7ff);
  opacity: 0.5;
  outline: none;
}

// Active state
.active-region {
  fill: var(--theme-tertiary, #6593ff);
  color: var(--theme-tertiary, #6593ff);

  &:hover,
  &:focus {
    fill: var(--theme-tertiary, #6593ff);
    color: var(--theme-tertiary, #6593ff);
    opacity: 1;
    outline: none;
  }
}
</style>
