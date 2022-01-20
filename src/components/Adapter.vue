<template>
  <div class="adapter">
    <table>
      <tr>
        <td rowspan="2" class="position">
          {{item.position}}
        </td>
        <td rowspan="2" class="nationality">
          <img :src=item.Driver.nationality class="flag" alt="flag" width="25" height="25">
        </td>
        <td class="driver">
          {{item.Driver.givenName.charAt(0)}}.
          {{item.Driver.familyName}}
        </td>
        <td rowspan="2" class="points">
          {{item.points}}pt
        </td>
      </tr>
      <tr>
        <td class="driver">
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

  .flag {
    border-radius: 5px;
    border: 1px solid grey !important;
    object-fit: cover;
  }

  .position {
    padding: 10px;
    width: 45px;
  }

  .nationality {
    padding-right: 15px;
  }

  .driver {
    width: 140px;
    text-align: left;
  }

  .points {
    width: 70px;
  }
</style>