<template>


  <section>
    <div>
      <label>Choose a year:</label>

      <select name="cars" id="cars" @change="onChange()">
        <!--<option v-for="i in 50" :key="i">{{ i }}</option>-->
        <option>2021</option>
        <option>2020</option>
        <option>2019</option>
        <option>2018</option>
      </select>
    </div><br><br>

    <div v-for="item in list" :key="item.driverId" class="content">
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
      year: 2021
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
