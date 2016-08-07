<template>
  <div class="brand">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Plotit!'
    }
  }
}
</script>

<style scoped>
  .brand {
    margin: 50px auto 20px;
  }
  h1 {
    margin: 0;
    padding: 0;
    color: #08D9D6;
  }
</style>
