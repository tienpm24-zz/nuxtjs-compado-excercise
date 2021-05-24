<template>
  <div class="searchbar-wrapper">
    <input
      class="searchbar-input"
      type="text"
      placeholder="Search products"
      @input="searchText"
    />
    <button class="searchbar-button" data-var="vsButton" aria-label="Search">
      <SearchIcon name="search" style="--width:24px; --height:24px" />
    </button>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Watch } from 'nuxt-property-decorator'
import SearchIcon from './Icon.vue'

@Component({
  components: { SearchIcon }
})
export default class ProductSearchbar extends Vue {
  debounce!: number
  @Watch('searchText')
  searchText(event: Event): void {
    const target = event.target as HTMLInputElement
    if (target.value.toString().length > 1) {
      this.debounceSearch(target.value)
    }
  }

  debounceSearch(searchText: string) {
    clearTimeout(this.debounce)
    this.debounce = window.setTimeout(
      () => this.$store.dispatch('handleSearchChange', searchText),
      600
    )
  }
}
</script>

<style lang="sass" scoped>
.searchbar-wrapper
  position: relative
  margin: auto 0
  max-width: 700px
  flex: 1 0 auto
  display: flex

.searchbar-input
  flex: 0 1 100%
  position: relative
  visibility: visible
  background: #f3f3f3
  cursor: text
  color: #333
  padding: 10px 50px 10px 44px
  border-radius: 100px
  height: 20px
  border: 0
  font-weight: 500
  font-size: 16px
  line-height: 1.5
  &:focus
    outline: none

.searchbar-button
  position: absolute
  top: 2px
  left: 0px
  padding: 6px 8px
  align-items: center
  border-radius: 100px
  background: transparent
  outline: 0
  cursor: pointer
  border: none
  border-width: 2px
  transition: 0.4s
  overflow: hidden
  &:focus
    outline: none
</style>
