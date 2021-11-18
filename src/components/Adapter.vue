<template>
  <div class="adapter">
    <table width="100%">
      <tr>
        <td rowspan="2">
          {{item.position}}
        </td>
        <td rowspan="2">
          <img :src=item.Driver.nationality alt="flag" width="25" height="20">
        </td>
        <td>
          {{item.Driver.givenName}}
          {{item.Driver.familyName}}
        </td>
        <td rowspan="2">
          {{item.points}}
        </td>
      </tr>
      <tr>
        <td>
          {{item.Constructors[0].name}}
          #{{item.Driver.permanentNumber}}
        </td>
      </tr>
    </table>
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