<template>


  <section>
    <div>
      <label>Choose a car:</label>

      <select name="cars" id="cars">
        <option value="volvo">Volvo</option>
        <option value="saab">Saab</option>
        <option value="mercedes">Mercedes</option>
        <option value="audi">Audi</option>
        <option value="Mattias">Mati stinkt</option>
      </select>
    </div>

    <div v-for="item in list" :key="item.driverId">
      {{ item.driverId }}
      <!--<Adapter item="{{item}}"/>-->
    </div>

  </section>

</template>

<script>
export default {
  //components: { Adapter },
  data() {
    return {
      list: []
    }
  },
  mounted() {
    fetch('http://ergast.com/api/f1/2021/drivers.json')
      .then(res => res.json())
      .then(data => this.list = data.MRData.DriverTable.Drivers)
      .catch(err => console.log(err.message))
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
