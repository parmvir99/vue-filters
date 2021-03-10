<template lang="html">
  <div class="card d-flex align-items-center" :class="{'featured' : item.featured}">
      <img :src="require(`@/assets/images/${item.logo}`)" alt="" class="logo-img">
    <div class="title">
      <div class="d-flex align-items-center">
         <strong class="mr-1 primary-font">{{ item.company }}</strong>
         <div class="badge-rounded-20 primary-bg mr-1" v-if="item.new">
           NEW!
         </div>
         <div class="badge-rounded-20 dark-bg" v-if="item.featured">
           FEATURED
         </div>
      </div>
      <h3>{{ item.position }}</h3>
      <div class="d-flex align-items-center light-cyan-font font-size-13 justify-space-between other-infos">
        <span>{{ item.postedAt }}</span>
        <span>{{ item.contract }}</span>
        <span>{{ item.location }}</span>
      </div>
    </div>
    <div class="filters ml-auto">
      <span class="badge-rounded-4 mr-1 light-cyan-bg" @click.prevent="addFilter(item.role)">{{ item.role }}</span>
      <span class="badge-rounded-4 mr-1 light-cyan-bg" @click.prevent="addFilter(item.level)">{{ item.level }}</span>
      <span class="badge-rounded-4 mr-1 light-cyan-bg" v-for="skill in item.languages" :key="skill" @click.prevent="addFilter(skill)">{{ skill }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    item: {
      required: true,
      type: Object
    }
  },
  methods: {
    addFilter (filter) {
      this.$emit('onAddFilter', filter)
    }
  }
}
</script>

<style lang="css">
</style>
