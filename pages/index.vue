<template>
  <div class="bg-gradient-to-r from-dark-gray to-black min-h-screen">
    <Header :title="title" />
    <Loading :loading="loading" v-if="loading" />
    <div v-else>
      <Departures :allDepartures="allDepartures" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      title: 'Departures',
      allDepartures: [],
      loading: false
    }
  },
  mounted() {
    this.getAllDepartures();
  },
  methods: {
    async getAllDepartures() {
      try {
        this.loading = true
        const res = await this.$axios.$get('https://a5bffa7f-92dc-454e-bd7b-41377007474b.mock.pstmn.io/departures')
        this.res = res
        this.allDepartures = res.allDepartures;
        this.loading = false
      } catch (e) {
        console.log('ERROR: ', e)
      }
    }
  }
}
</script>

<style>
body {
  font-family: 'Helvetica', Arial, sans-serif
}
</style>
