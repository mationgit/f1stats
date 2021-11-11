<template>
  <div class="adapter">
    <img src={{item.Driver.nationality}} alt="flag" width="20" height="20">
    {{item.Driver.permanentNumber}}
    {{item.Driver.givenName}}
    {{item.Driver.familyName}}
    <br>
    {{item.Driver.dateOfBirth}}
    {{item.Constructors[0].name}}
    {{item.Driver.nationality}}
    
  </div>
  <hr>
</template>

<script>
export default {
  props: {
    item: {}
  },
  data () {
    
  },
  methods: {
    fetchCountryFlag: function() {
    }
  },
  mounted() {
    this.fetchCountryFlag()
  }
}
</script>

<style>
  .adapter {
    transition: 0.5s;
    cursor: default;
  }

  .adapter:hover {
    background-color: lightcoral;
    border-radius: 10px;
  }

  hr {
    border-top: 1px solid darkgray;
  }
</style>