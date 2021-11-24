<template>
  <section>
    <div>
      <div class="w3-row">
        <div class="w3-container w3-col" style="width:30%"></div>
        <img class="w3-image w3-col" style="width:40%" :src="require('@/assets/logo.png')" v-on:click="back">
      </div>
      <label class = "heading1">Stats</label><br>
      <br>
    </div>
    <div class="container" id="list">
      <label>Year: </label>
      <div class="w3-row"></div>
      <div class="w3-col w3-container"></div>
      <select class="w3-select content" name="year" id="year" v-model="year" @change="onChange()">
        <!--<option v-for="i in 50" :key="i">{{ i }}</option>-->
        <option v-for="year in years" :key="year">{{year}}</option>
      </select><br><br>
      <label>Search: </label>
      <div class="w3-row">
        <div class="w3-col w3-container"></div>
        <input class="w3-input w3-border w3-round content" v-model="searchTerm" type="text">
      </div>
      <br><br>
      <div v-for="item in filterByTerm" :key="item.Driver.driverId" class="content">
        <div v-on:click="showDetails(item)">
          <Adapter :item="item" />
        </div>
      </div>
    </div>
    <div class="container" id="detail">
      <div v-if="driver.Driver && driver.Constructors" class="content">
        <h3>Driver</h3>
        <ul>
          <li>
            <strong>Name: </strong>
            <span>{{driver.Driver.givenName + " " + driver.Driver.familyName}}</span>
          </li>
          <li>
            <strong>Birth: </strong>
            <span>{{driver.Driver.dateOfBirth}}</span>
          </li>
          <li>
            <strong>Car Number: </strong>
            <span>{{driver.Driver.permanentNumber}}</span>
          </li>
          <li>
            <strong>Driver Code: </strong>
            <span>{{driver.Driver.code}}</span>
          </li>
        </ul>
        <img :src=driver.Driver.nationality alt="flag" class="w3-round-large w3-border" height="50">
        <h3>Results {{year}}</h3>
        <ul>
          <li>
            <strong>Position: </strong>
            <span>{{driver.position}}</span>
          </li>
          <li>
            <strong>Points: </strong>
            <span>{{driver.points}}</span>
          </li>
          <li>
            <strong>Wins: </strong>
            <span>{{driver.wins}}</span>
          </li>
        </ul>
        <h3>Constructors</h3>
        <table class="w3-table w3-striped w3-border">
          <thead>
            <tr class="w3-red">
              <td>
                Name
              </td>
              <td>
                Nationality
              </td>
            </tr>
          </thead>
          <tr v-for="constructor in driver.Constructors" :key="constructor.constructorId">
            <td>
              {{constructor.name}}
            </td>
            <td>
              {{constructor.nationality}}
            </td>
          </tr>
        </table><br>
        <button class="w3-btn w3-red w3-round-large" @click="back">Back</button>
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
      years: this.fillYears(),
      year: new Date().getFullYear(),
      searchTerm: "",
      driver: {}
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
        },
        showDetails: function(item) {
          this.driver = item
          document.getElementById('list').style.display = 'none'
          document.getElementById('detail').style.display = 'block'
        },
        back: function() {
          document.getElementById('detail').style.display = 'none'
          document.getElementById('list').style.display = 'block'
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

  ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
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

  #list {
    display: block;
  }

  #detail {
    display: none;
  }
</style>
