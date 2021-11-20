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
        <select class="w3-select w3-col" name="year" style="width:20%" id="year" @change="onChange()">
          <!--<option v-for="i in 50" :key="i">{{ i }}</option>-->
          <option v-for="year in years" :key="year">{{year}}</option>
        </select><br><br>
      </div>
      <label>Search: </label>
      <div class="w3-row">
        <div class="w3-col w3-container" style="width:40%"> </div>
        <input class="w3-input w3-border w3-round w3-col" style="width:20%" v-model="searchTerm" type="text">
      </div>
      <br><br>
    <div v-for="item in filterByTerm" :key="item.Driver.driverId" class="content">
        <Adapter :item="item" />
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
      years: this.fillYears(),
      year: 2021,
      searchTerm: ""
    }
  },
  computed: {
    filterByTerm() {
      return this.list.filter(item => {
        return (item.Driver.familyName + item.Driver.givenName + item.Constructors[0].name).toLowerCase().includes(this.searchTerm.toLowerCase());
      });
    }
  },
  mounted() {
    this.onChange()
  },
  methods: 
      {
        onChange: function() {
          fetch('https://ergast.com/api/f1/' + document.getElementById('year').value + '/driverStandings.json')
          .then(res => res.json())
          .then(data => {
            this.list = data.MRData.StandingsTable.StandingsLists[0].DriverStandings
            this.list.forEach(element => {
              element.Driver.nationality = this.getCountryCode(element.Driver.nationality)
            })
            
          })
        },
        getCountryCode: function(nationality) {
          let cc = ''
          this.countryCodes.forEach(country => {
            if (country.nationality.split(', ').includes(nationality)) {
              cc = country.alpha_2_code.toLowerCase()
              return
            }
          })
          return 'https://flagpedia.net/data/flags/normal/' + cc + '.png'
        },
        fillYears: function() {
          var years = []
          for (var i = new Date().getFullYear(); i >= 1950; i--)
            years.push(i)
          return years
        }
      }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="css">
  @import "https://cdnjs.cloudflare.com/ajax/libs/w3-css/4.1.0/w3.css";

  .content {
    width: 25%;
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

  @media only screen and (max-width: 1350px) {
    .content {
      width: 30%;
    }
  }

  @media only screen and (max-width: 1100px) {
    .content {
      width: 35%;
    }
  }

  @media only screen and (max-width: 900px) {
    .content {
      width: 45%;
    }
  }

  @media only screen and (max-width: 700px) {
    .content {
      width: 55%;
    }
  }

  @media only screen and (max-width: 600px) {
    .content {
      width: 65%;
    }
  }

  @media only screen and (max-width: 500px) {
    .content {
      width: 75%;
    }
  }

  @media only screen and (max-width: 450px) {
    .content {
      width: 95%;
    }
  }
</style>
