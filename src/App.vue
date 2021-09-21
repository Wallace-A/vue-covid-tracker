<template>
  <Header></Header>
    <div class="container m-auto">
      <main v-if="!loading">
        <DataTitle :text="title" :dataDate="dataDate" />

        <DataBoxes :stats="stats" />
        <!-- @get-country is called in CountrySelect and passes a country to getCountryData function and calls it -->
        <CountrySelect @get-country="getCountryData" :countries="countries"/>

        <button 
        @click="clearCountryData"
        v-if="stats.Country"
        class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
          Clear Country
        </button>
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
import DataBoxes from "./components/DataBoxes"
import CountrySelect from "./components/CountrySelect"

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
    DataTitle,
    DataBoxes,
    CountrySelect
  },
  methods: {
    async fetchCovidData() {
      const response = await fetch("https://api.covid19api.com/summary")
      const data = await response.json()
      return data
    },
    //recieves country from CountrySelect
    getCountryData(country){
      this.stats = country
      this.title = country.Country
    },
    async clearCountryData(){
      this.loading = true;
      const data = await this.fetchCovidData();
      this.title = "Global";
      this.stats = data.Global;
      this.loading = false;
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
