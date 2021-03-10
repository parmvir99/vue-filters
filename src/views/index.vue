<template>
  <div class="home" v-if="data">
    <div class="top-banner">
      <img src="@/assets/images/bg-header-desktop.svg" alt="" class="primary-bg banner-image">
    </div>
    <div class="container">
      <div class="card filter-card d-flex align-items-center" v-if="filters.length >= 1">
        <div class="">
          <div class="mr-1 mb-1 inline-flex" v-for="(filter, idx) in filters" :key="idx" :class="{'haveFilter' : tempFilter === filter}">
            <span class="badge-rounded-4 light-cyan-bg mb-0"> {{ filter }} </span>
            <span class="filter-remove" @click="removeFilter(idx)"><i class="mdi mdi-close" aria-hidden="true"></i></span>
          </div>
        </div>
        <div class="clear-filters" @click.prevent="clearFilters()">
          <span href="#" class="primary-font">Clear</span>
        </div>
      </div>
      <jobCard v-for="item in data" :key="item.id" :item="item" @onAddFilter="addFilter"/>
    </div>
  </div>
</template>

<script>
import data from '@/data/data'
import { each, filter } from 'lodash/collection'
import { indexOf } from 'lodash/array'
import { clone } from 'lodash/lang'
import jobCard from '@/components/job-card'
export default {
  name: 'Home',
  data () {
    return {
      data: data,
      originalData: null,
      tempFilter: null,
      filters: []
    }
  },
  components: {
    jobCard
  },

  methods: {
    addFilter (filter) {
      if (indexOf(this.filters, filter) > -1) {
        this.tempFilter = filter
      } else {
        this.filters.push(filter)
        this.runFilter(this.filters)
      }
    },
    runFilter (addedFilters) {
      this.data = filter(this.originalData, (item, key) => {
        let searchRatings = 0
        each(addedFilters, (filter, key) => {
          if (indexOf(item.languages, filter) >= 0)searchRatings++
          if (filter === item.level) searchRatings++
          if (filter === item.role) searchRatings++
        })
        if (searchRatings > addedFilters.length - 1) {
          return item
        }
      })

      // Not so strict filtering
      // this.data = filter(this.originalData, (item) => {
      //   if (intersection(addedFilters, item.languages).length > 0) {
      //     return item
      //   } else if (indexOf(addedFilters, item.level) > -1) {
      //     return item
      //   } else if (indexOf(addedFilters, item.role) > -1) {
      //     return item
      //   }
      // })
    },
    removeFilter (idx) {
      this.filters.splice(idx, 1)
      if (this.filters.length > 0) this.runFilter(this.filters)
      else this.data = this.originalData
    },
    clearFilters () {
      this.filters = []
      this.data = this.originalData
    }
  },
  mounted () {
    this.originalData = clone(this.data)
  }
}
</script>
