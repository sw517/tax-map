<template>
  <div id="app" class="tax-map-app">
    <div class="tax-map-app__header">
      <div class="tax-map-app__header__search">
        <SearchInput
          :value="searchInput"
          :placeholder="dataStructure.searchPlaceholderText"
          @input="onSearchInputChange"
        />
        <ListDropdown
          :items="dropdownListItems"
          class="tax-map-app__header__search-dropdown"
          @click="onListDropdownClick"
        />
      </div>
      <SocialShare
        share-url="https://www.google.com/"
        :share-text="dataStructure.shareText"
        :share-to-text="dataStructure.socialLabelText"
        class="tax-map-app__header__social"
      />
    </div>
    <div class="tax-map-app__body">
      <div class="tax-map-app__body__info-wrap">
        <InfoPopup v-show="!showInfoPopup">
          {{ dataStructure.selectRegionText }}
        </InfoPopup>
        <TaxInfoPopup
          v-show="showInfoPopup"
          :title="selectedRegionData.title"
          :bullets="selectedRegionData.bullets"
          :link="selectedRegionData.link"
          class="tax-map-app__body__info-popup"
          @close="onInfoPopupClose"
        />
      </div>
      <div class="tax-map-app__body__map-wrap">
        <EuropeMap
          :activeRegionId="selectedRegionId"
          :regions="dataStructure.regions"
          class="tax-map-app__body__map"
          @path-click="onMapClick"
        />
      </div>
    </div>
  </div>
</template>

<script>
// Components
import EuropeMap from '@/components/dynamic-maps/EuropeMap'
import InfoPopup from '@/components/info-popups/InfoPopup'
import TaxInfoPopup from '@/components/info-popups/TaxInfoPopup'
import SearchInput from '@/components/forms/SearchInput'
import ListDropdown from '@/components/forms/ListDropdown'
import SocialShare from '@/components/SocialShare'
// Data
import dataStructure from '@/assets/data/tax-map-structure.json'

const blankRegion = () => ({
  title: '',
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
      dataStructure,
      dropdownListItems: [],
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
    this.dropdownListItems = this.dataStructure.regions
  },
  methods: {
    handleSearchQuery() {
      const search = this.searchInput.toLowerCase()
      const searchedRegions = this.dataStructure.regions.filter(
        ({ countryName, countryCode }) => {
          return (
            countryName.toLowerCase().includes(search) ||
            countryCode.toLowerCase().includes(search)
          )
        }
      )

      if (searchedRegions) {
        this.dropdownListItems = searchedRegions
      } else {
        this.dropdownListItems = this.dataStructure.regions
      }
    },
    isValidRegion(regionData) {
      return (
        typeof regionData === 'object' &&
        regionData.title &&
        regionData.countryCode &&
        regionData.link
      )
    },
    onInfoPopupClose() {
      this.resetSelectedRegionData()
      this.showInfoPopup = false
    },
    onListDropdownClick(region) {
      this.searchInput = region.countryName
      this.onMapClick(region.countryCode)
      // Hide dropdown by removing focus
      if (document.activeElement) document.activeElement.blur()
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
      const region = this.dataStructure.regions.find(({ countryCode }) =>
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
.tax-map-app {
  border: 1px solid black;
  padding: 10px;

  @media screen and (min-width: 680px) {
    padding: 30px;
  }

  &__header {
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;

    @media screen and (min-width: 680px) {
      flex-direction: row;
      align-items: start;
      justify-content: space-between;
      margin-bottom: 30px;
    }

    &__social {
      margin-bottom: 15px;
      order: -1;

      @media screen and (min-width: 680px) {
        order: initial;
        margin-bottom: 0;
      }
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
      width: 500px;

      @media screen and (min-width: 768px) {
        width: 100%;
        min-width: 500px;
      }
    }

    &__map-wrap {
      overflow: auto;

      @media screen and (min-width: 768px) {
        flex-grow: 1;
      }
    }

    &__info-wrap {
      margin-bottom: 10px;

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
