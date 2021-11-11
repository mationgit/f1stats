<template>
  <section>
    <div>
      <label>Year: </label>

      <select class="w3-select" name="cars" id="cars" @change="onChange()">
        <!--<option v-for="i in 50" :key="i">{{ i }}</option>-->
        <option>2021</option>
        <option>2020</option>
        <option>2019</option>
        <option>2018</option>
      </select><br><br>
      <label>Search: </label>
      <input class="w3-input w3-border w3-round" v-model="searchTerm" type="text">
    </div><br><br>

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
  methods: {
    onChange: function() {
      
      fetch('https://ergast.com/api/f1/' + document.getElementById('cars').value + '/driverStandings.json')
      .then(res => res.json())
      .then(data => {
        this.list = data.MRData.StandingsTable.StandingsLists[0].DriverStandings
        console.log(data.MRData.StandingsTable.StandingsLists[0].DriverStandings)
      })
      .catch(err => console.log(err.message))
    }
  },
  mounted() {
    this.onChange()
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
</style>
