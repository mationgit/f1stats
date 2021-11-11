<template>
  <section>
    <div class = "container"> 
      <div class="w3-row">
        <div class="w3-container w3-col" style="width:30%"></div>
        <img class="w3-image w3-col" style="width:40%" :src="require('@/assets/logo.png')">
      </div>
      <label class = "heading1">Stats</label><br>
      <br>
        <label>Year: </label>
      <div class="w3-row"></div>
        <div class="w3-col w3-container" style="width:40%"> </div>
        <select class="w3-select w3-col" name="cars" style="width:20%" id="cars" @change="onChange()">
          <!--<option v-for="i in 50" :key="i">{{ i }}</option>-->
          <option>2021</option>
          <option>2020</option>
          <option>2019</option>
          <option>2018</option>
        </select><br><br>
      </div>
      <label>Search: </label>
      <div class="w3-row">
        <div class="w3-col w3-container" style="width:20%"> </div>
        <input class="w3-input w3-border w3-round w3-col" style="width:60%" v-model="searchTerm" type="text">
      </div>
      <br><br>
    <div v-for="item in filterByTerm" :key="item.Driver.driverId" class="content">
      <div class="w3-row">
        <div class="w3-container w3-col" style="width:10%"></div>
        <Adapter class="w3-col" style="width:80%" :item="item" />
      </div>
    </div>
    



  </section>

</template>

<script>
import Adapter from './Adapter.vue'
export default {
  components: { Adapter },
  data() {
    return {
      countryCodes: require('../assets/countries.json'),
      list: [],
      year: 2021,
      searchTerm: ""
    }
  },
  computed: {
    filterByTerm() {
      return this.list.filter(item => {
        return item.Driver.familyName.toLowerCase().includes(this.searchTerm.toLowerCase());
      });
    }
  },
  methods: 
      {
        onChange: function() {
          console.log("test")
          fetch('https://ergast.com/api/f1/' + document.getElementById('cars').value + '/driverStandings.json')
          .then(res => res.json())
          .then(data => {
            this.list = data.MRData.StandingsTable.StandingsLists[0].DriverStandings
            console.log(this.list)
            
            this.list.forEach(element => {
              element.Driver.nationality = this.getCountryCode(element.Driver.nationality)
            })
            
          })
        },
        mounted() {
          this.onChange()
        },
        getCountryCode: function(nationality) {
          let cc = ''
          this.countryCodes.forEach(country => {
            if (country.nationality.includes(nationality)) {
              cc = country.alpha_2_code.toLowerCase()
            }
          })
          console.log(cc)
          return 'https://flagpedia.net/data/flags/normal/' + cc + '.png'
        }
      }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="css">
  @import "https://cdnjs.cloudflare.com/ajax/libs/w3-css/4.1.0/w3.css";

  .content {
    width: 40%;
    margin-left: auto;
    margin-right: auto;
  }
  .container {
    width: 100%;  
    height: 100%;  
    object-fit: cover; 
  }
  .heading1{
    font-family: Tahoma, sans-serif;
    font-size: 200%;
    color: red;
    font-weight: bold;
  }
</style>
