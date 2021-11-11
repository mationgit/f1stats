<template>
  <div class="adapter">
    {{item.Driver.permanentNumber}}
    {{item.Driver.givenName}}
    {{item.Driver.familyName}}
    <br>
    {{item.Driver.dateOfBirth}}
    {{item.Driver.nationality}}
    {{item.Constructors[0].name}}
  </div>
  <hr>
</template>

<script>
export default {
  props: {
    item: {}
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