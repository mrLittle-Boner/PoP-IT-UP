<template class="expand">
  <section class="start" v-show="!modal">
    <button 
      class="start__button button"
      @click="modal = true">
      Налоговый вычет
    </button>
  </section>
  <Calculator 
    v-show="modal" 
    @close="closeModal()"
  /> 
</template>

<script>
import Calculator from './components/Calculator.vue'

export default {
  name: 'App',
  components: {
    Calculator
  },
  data() {
    return{
      modal: false,
    }
  },
  methods: {
    closeModal() {
      this.modal = false;
    }
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

.start__button {
  padding: 16px 32px;
  @media screen and (max-width: 425px) {
    padding: 12px 24px;
  }
}

</style>
