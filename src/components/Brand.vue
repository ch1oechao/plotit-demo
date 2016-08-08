<template>
  <div class="brand">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'PlotIt !'
    }
  }
}
</script>

<style scoped>
  .brand {
    margin: 30px auto 10px;
    font-family: 'Lobster', cursive;
  }
  h1 {
    margin: 0;
    padding: 0;
    color: #08D9D6;
    font-size: 42px;
    letter-spacing: .1em;
  }
</style>
