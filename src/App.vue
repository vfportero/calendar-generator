<template>
  <div>
    <template v-for="d in Array(12).fill().map((x,i)=>i + 1)" :key="d">
      <Month :month="d" />
    </template>
  </div>
</template>

<script>
import Month from './components/Month.vue'

export default {
  name: 'App',
  components: {
    Month
  }
}
</script>

<style>
#app {
  font-family: 'Lora', serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
@media print {
  #app {
    max-width: fit-content;
  }
}
</style>
