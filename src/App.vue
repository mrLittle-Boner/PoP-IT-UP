<template>
  <section class="start">
    <button class="start__button button button--big">Налоговый вычет</button>
  </section>
  <Calculator /> 
</template>

<script>
import Calculator from './components/Calculator.vue'

export default {
  name: 'App',
  components: {
    Calculator
  }
}
</script>

<style lang="scss">
#app {
  font-family: "Lab Grotesque-Regular", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.start {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;

  background: $gradient-red;
  box-shadow: 0px -0.11px 16.9495px rgba(183, 187, 225, 0.33);
}
</style>
