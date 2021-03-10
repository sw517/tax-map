<template>
  <div id="app" class="tax-map-app">
    <div class="tax-map-app__header">
      <div class="tax-map-app__header__search">
        <SearchInput :value="searchInput" @input="onSearchInputChange" />
        <ListDropdown
          :items="dropdownListItems"
          class="tax-map-app__header__search-dropdown"
          @click="onListDropdownClick"
        />
      </div>
      <SocialShare />
    </div>
    <div class="tax-map-app__body">
      <div class="tax-map-app__body__map-wrap">
        <EuropeMap
          :activeRegionId="selectedRegionId"
          :regions="regionData"
          class="tax-map-app__body__map"
          @path-click="onMapClick"
        />
      </div>
      <div class="tax-map-app__body__info-wrap">
        <InfoPopup v-show="!showInfoPopup">
          Select a region on the map to find out more info...
        </InfoPopup>
        <TaxInfoPopup
          v-show="showInfoPopup"
          :region="selectedRegionData.region"
          :bullets="selectedRegionData.bullets"
          :link="selectedRegionData.link"
          class="tax-map-app__body__info-popup"
          @close="onInfoPopupClose"
        />
      </div>
    </div>
  </div>
</template>

<script>
import EuropeMap from '@/components/dynamic-maps/EuropeMap'
import InfoPopup from '@/components/info-popups/InfoPopup'
import TaxInfoPopup from '@/components/info-popups/TaxInfoPopup'
import SearchInput from '@/components/forms/SearchInput'
import ListDropdown from '@/components/forms/ListDropdown'
import SocialShare from '@/components/SocialShare'

const blankRegion = () => ({
  region: '',
  countryCode: '',
  bullets: [],
  link: {
    href: '',
    text: '',
  },
})

export default {
  name: 'App',
  components: {
    EuropeMap,
    InfoPopup,
    TaxInfoPopup,
    SearchInput,
    ListDropdown,
    SocialShare,
  },
  data() {
    return {
      dropdownListItems: [],
      regionData: [
        {
          region: 'UK',
          countryCode: 'gb',
          bullets: [
            'Taxes apply only to cashed-out balances',
            'Real Cryptocurrency holdings incur in a -5%',
            'Declared annually',
            'Taxes apply only to cashed-out balances.',
            'Real Cryptocurrency holdings incur in a -5%',
          ],
          link: {
            href: '/',
            text: 'Learn How to Do your Crypto Taxes in the UK',
          },
        },
        {
          region: 'France',
          countryCode: 'fr',
          bullets: [
            'Real Cryptocurrency holdings incur in a -5%',
            'Taxes apply only to cashed-out balances',
            'Declared annually',
            'Taxes apply only to cashed-out balances.',
          ],
          link: {
            href: '/',
            text: 'Learn How to Do your Crypto Taxes in France',
          },
        },
        {
          region: 'Italy',
          countryCode: 'it',
          link: {
            href: '/',
            text: 'Learn How to Do your Crypto Taxes in France',
          },
        },
      ],
      searchInput: '',
      selectedRegionId: '',
      selectedRegionData: blankRegion(),
      showInfoPopup: false,
    }
  },
  watch: {
    searchInput(value) {
      if (!value) return
    },
  },
  created() {
    this.dropdownListItems = this.regionData
  },
  methods: {
    handleSearchQuery() {
      const search = this.searchInput.toLowerCase()
      const searchedRegions = this.regionData.filter(
        ({ region, countryCode }) => {
          return (
            region.toLowerCase().includes(search) ||
            countryCode.toLowerCase().includes(search)
          )
        }
      )

      console.log(searchedRegions)

      if (searchedRegions) {
        this.dropdownListItems = searchedRegions
      } else {
        this.dropdownListItems = this.regionData
      }
    },
    isValidRegion(regionData) {
      return (
        typeof regionData === 'object' &&
        regionData.region &&
        regionData.countryCode &&
        regionData.link
      )
    },
    onInfoPopupClose() {
      this.resetSelectedRegionData()
      this.showInfoPopup = false
    },
    onListDropdownClick(region) {
      this.searchInput = region.region
      this.onMapClick(region.countryCode)
    },
    onMapClick(regionId) {
      this.selectedRegionId = regionId
      this.setSelectedRegionData()
      this.showInfoPopup = this.isValidRegion(this.selectedRegionData)
    },
    onSearchInputChange(value) {
      this.searchInput = value
      this.handleSearchQuery()
    },
    resetSelectedRegionData() {
      this.selectedRegionId = ''
      this.selectedRegionData = blankRegion()
    },
    setSelectedRegionData() {
      const region = this.regionData.find(({ countryCode }) =>
        countryCode.includes(this.selectedRegionId)
      )

      if (this.isValidRegion(region)) {
        this.selectedRegionData = region
      } else {
        this.resetSelectedRegionData()
      }
    },
  },
}
</script>

<style lang="scss">
// Todo: Remove root scope
:root {
  --theme-tertiary: #ffb400;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.tax-map-app {
  border: 1px solid black;
  padding: 10px;

  @media screen and (min-width: 680px) {
    padding: 30px;
  }

  &__header {
    @media screen and (min-width: 680px) {
      display: flex;
      align-items: start;
      justify-content: space-between;
      margin-bottom: 30px;
    }

    &__search {
      position: relative;

      // Only show dropdown when focus is on search input.
      &:focus-within .tax-map-app__header__search-dropdown {
        display: block;
      }

      &-dropdown {
        display: none;
        position: absolute;
        left: 0;
        top: 100%;
        width: 100%;
        z-index: 10;
      }
    }
  }

  &__body {
    @media screen and (min-width: 680px) {
      display: flex;
      align-items: start;
    }

    &__map {
      width: 680px;
      height: 520px;

      @media screen and (min-width: 1024px) {
        width: auto;
        height: auto;
      }
    }

    &__map-wrap {
      overflow: scroll;

      @media screen and (min-width: 1024px) {
        overflow: initial;
        flex-grow: 1;
      }
    }

    &__info-wrap {
      @media screen and (min-width: 680px) {
        order: -1;
        margin-right: 30px;
        min-width: 300px;
        max-width: 300px;
      }
    }
  }
}
</style>
