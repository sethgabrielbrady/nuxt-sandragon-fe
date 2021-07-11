<template>
  <div class="container">
    <Header />
    <div>
      <p class="h3 title mx-auto max-width-1">
        Sandragon
      </p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Header from '@/components/Header.vue'

export default {
  components: {
    Header
  },
  data () {
    return {
      response: 'Initial State'
    }
  },
  created () {
    return this.asyncData()
  },
  methods: {
    async asyncData () {
      const res = await axios.get(this.$axios.defaults.baseURL)
      this.response = res.data
    }
  }
}
</script>

<style scoped>
</style>
