<template>


  <section>
    <div>
      <label>Year: </label>

      <select name="cars" id="cars" @change="onChange()">
        <!--<option v-for="i in 50" :key="i">{{ i }}</option>-->
        <option>2021</option>
        <option>2020</option>
        <option>2019</option>
        <option>2018</option>
      </select><br><br>
      <label>Search: </label>
      <input v-model="searchTerm" type="text">
    </div><br><br>

    <div v-for="item in filterByTerm" :key="item.driverId" class="content">
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
        return item.familyName.toLowerCase().includes(this.searchTerm.toLowerCase());
      });
    }
  },
  methods: {
    onChange: function() {
      
      fetch('https://ergast.com/api/f1/' + document.getElementById('cars').value + '/drivers.json')
      .then(res => res.json())
      .then(data => this.list = data.MRData.DriverTable.Drivers)
      .catch(err => console.log(err.message))
    }
  },
  mounted() {
    this.onChange()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .content {
    width: 40%;
    margin-left: auto;
    margin-right: auto;
  }
</style>
