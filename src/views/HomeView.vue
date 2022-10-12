<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <CalculationModule class="cal"/>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import CalculationModule from '../components/CalculationModule.vue'; // @ is an alias to /src

@Options({
  components: {
    CalculationModule,
  },
})
export default class HomeView extends Vue {}
</script>

<style lang="scss">
  .cal {
    margin: 5rem 30rem
  }
  
</style>
