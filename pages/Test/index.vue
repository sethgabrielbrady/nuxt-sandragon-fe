<template>
  <div v-show="loaded">
    {{ $localStorageLoaded }}
    {{ $sessionStorageLoaded }}
    {{ $store.state.localStorage.anyValues }}
    {{ $store.state.sessionStorage.anyValues }}
  </div>
</template>

<script>
export default {
  data () {

  },
  computed: {
    loaded () {
      return this.$store.state.localStorage.status && this.$store.state.sessionStorage.status
    }
  }
}
</script>
