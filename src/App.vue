<template>
  <Header></Header>
    <div class="container"></div>
</template>

<script>
import Header from "./components/Header"

export default {
  name: 'App',
  components: {
    Header
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
    console.log(data);
  }
}
</script>

<style>
#app {
  
}
</style>
