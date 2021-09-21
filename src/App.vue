<template>
  <Header></Header>
    <div class="container m-auto">
      <main v-if="!loading">
        <DataTitle :text="title" :dataDate="dataDate" />
      </main>

      <main class="flex flex-col align-center justify-center text-center" v-else>
        <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching Data...</div>
        <img :src="loadingImage" class="w-24 m-auto" alt="" />
      </main>
    </div>
</template>

<script>
import Header from "./components/Header"
import DataTitle from "./components/DataTitle"

export default {
  name: 'App',
  data() {
    return {
      // app variables
      loading: true,
      title: "Global",
      dataDate: "",
      stats: {},
      countries: [],
      loadingImage: require("./assets/hourglass.gif")
    }
  },
  components: {
    Header,
    DataTitle
  },
  methods: {
    async fetchCovidData() {
      const response = await fetch("https://api.covid19api.com/summary")
      const data = await response.json()
      return data
    }
  },
  //set as async because it's a promise
  async created(){
    const data = await this.fetchCovidData();

    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  }
}
</script>

<style>
#app {
  
}
</style>
