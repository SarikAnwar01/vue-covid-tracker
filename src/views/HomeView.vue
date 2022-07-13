<template>
  <main v-if="!loading">
   <DataTitle :text="title" :dataDate="dataDate"/>
    <DataBoxes :stats="stats"/>

    <CountrySelect @country-changed="getCountryData" :countries="countries"/>

    <button @click="clearCountry"
    v-if="stats.Country" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">Get Global Data</button>
  
  </main>
  <main class="flex flex-col align-center justify-center text-center " v-else>
    <div class="text-grey-500 text-3xl mt-10 mb-6">
      Fetching Data ...
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="loading">

  </main>
</template>

<script>
// @ is an alias to /src
import DataTitle from '@/components/DataTitle';
import DataBoxes from '@/components/DataBoxes';
import CountrySelect from '@/components/CountrySelect';

export default {
  name: 'HomeView',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
   
  },
  data() {
    return {
     loading:true,
     title:"Global",
     dataDate:"",
     stats:{},
     countries:[],
     loadingImage:require("../assets/hourglass.gif"),
    }
  },
  methods:{
    async fetchCovidData(){
      const res = await fetch('https://api.covid19api.com/summary');
      const data = await res.json();
     return data;

    },
    async getCountryData(country){
      this.title = country.Country;
      this.dataDate = country.Date;
      this.stats = country;
      this.loading = false;
    },
    async clearCountry(){
      this.loading = true;

     const data = await this.fetchCovidData();
     this.title = "Global";
      this.dataDate = data.Date;
      this.stats = data.Global;
     this.loading = false;

    },
  },
  async created() {
    // this.getData();
   const data =await this.fetchCovidData();
    this.dataDate = data.Date;
    this.stats=data.Global;
    this.countries=data.Countries;
    this.loading=false;
    
  },
}
</script>
