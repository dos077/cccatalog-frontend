<template>
  <div :class="{ 'scroll-y': filtersExpandedByDefault }">
    <div>
      <h4
        class="padding-top-big padding-left-big padding-right-normal is-inline-block"
      >
        Filter results by
      </h4>

      <button
        type="button"
        class="button is-text tiny is-paddingless margin-top-big margin-right-small report is-shadowless is-pulled-right"
        @click="onToggleSearchGridFilter()"
      >
        <span class="has-color-tomato is-hidden-touch">Hide filters</span>
        <span class="margin-right-normal is-size-5 is-hidden-desktop">
          <i class="icon cross" />
        </span>
      </button>
    </div>
    <form
      :class="{
        'filters-form': true,
      }"
      role="list"
    >
      <filter-check-list
        role="listitem"
        :options="filters.licenseTypes"
        :disabled="licenseTypesDisabled"
        title="Use"
        filterType="licenseTypes"
        @filterChanged="onUpdateFilter"
      />
      <filter-check-list
        role="listitem"
        v-if="activeTab == 'image'"
        :options="filters.licenses"
        :disabled="licensesDisabled"
        title="Licenses"
        filterType="licenses"
        @filterChanged="onUpdateFilter"
      />
      <filter-check-list
        role="listitem"
        v-if="renderProvidersFilter && activeTab == 'image'"
        :options="filters.providers"
        title="Sources"
        filterType="providers"
        @filterChanged="onUpdateFilter"
      />
      <filter-check-list
        role="listitem"
        v-if="activeTab == 'image'"
        :options="filters.categories"
        title="Image Type"
        filterType="categories"
        @filterChanged="onUpdateFilter"
      />
      <filter-check-list
        role="listitem"
        v-if="activeTab == 'image'"
        :options="filters.extensions"
        title="File Type"
        filterType="extensions"
        @filterChanged="onUpdateFilter"
      />
      <filter-check-list
        role="listitem"
        v-if="activeTab == 'image'"
        :options="filters.aspectRatios"
        title="Aspect Ratio"
        filterType="aspectRatios"
        @filterChanged="onUpdateFilter"
      />
      <filter-check-list
        role="listitem"
        v-if="activeTab == 'image'"
        :options="filters.sizes"
        title="Image Size"
        filterType="sizes"
        @filterChanged="onUpdateFilter"
      />

      <div
        v-if="activeTab == 'image'"
        class="margin-normal filter-option small-filter margin-bottom-normal"
      >
        <label aria-label="search by creator">
          <input
            aria-label="search by creator"
            type="checkbox"
            :checked="filters.searchBy.creator"
            @change="onUpdateSearchByCreator"
          />
          Search by Creator
        </label>
      </div>
    </form>

    <div
      class="margin-big padding-bottom-normal clear-filters is-hidden-touch"
      v-if="isFilterApplied"
    >
      <button class="button tiny" @click="onClearFilters">
        Clear filters
      </button>
    </div>

    <div
      v-if="isFilterApplied"
      class="has-background-white padding-big is-hidden-desktop has-text-centered"
    >
      <button class="button tiny margin-right-normal" @click="onClearFilters">
        Clear filters
      </button>
      <button
        class="button is-primary tiny"
        @click="onToggleSearchGridFilter()"
      >
        Show results
      </button>
    </div>
  </div>
</template>

<script>
import { ExperimentData } from '@/abTests/experiments/filterExpansion'
import FilterCheckList from './FilterChecklist'

export default {
  name: 'filters-list',
  props: [
    'filters',
    'isFilterApplied',
    'licenseTypesDisabled',
    'licensesDisabled',
    'renderProvidersFilter',
  ],
  components: {
    FilterCheckList,
  },
  methods: {
    onUpdateFilter({ code, filterType }) {
      this.$emit('onUpdateFilter', { code, filterType })
    },
    onUpdateSearchByCreator() {
      this.$emit('onUpdateSearchByCreator')
    },
    onToggleSearchGridFilter() {
      this.$emit('onToggleSearchGridFilter')
    },
    onClearFilters() {
      this.$emit('onClearFilters')
    },
  },
  computed: {
    activeTab() {
      return this.$route.path.split('search/')[1] || 'image'
    },
    /**
     * Check if a filter experiment is active, and if the current case is 'expanded'.
     * Show filters collapsed by default
     */
    filtersExpandedByDefault() {
      const experiment = this.$store.state.experiments.find(
        (exp) => exp.name === ExperimentData.EXPERIMENT_NAME
      )
      return experiment
        ? experiment.case === ExperimentData.FILTERS_EXPANDED_CASE
        : false
    },
  },
}
</script>

<style lang="scss" scoped>
.scroll-y {
  overflow-y: scroll;
  height: calc(100vh - 84px);
}
</style>
