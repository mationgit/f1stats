<template>
  <div class="adapter">
    {{item.permanentNumber}}
    {{item.givenName}}
    {{item.familyName}}
    <br>
    {{item.dateOfBirth}}
    {{item.nationality}}
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