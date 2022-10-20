<template>
  <div class="home">
    <HomeRideCalculator class="cal"/>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import HomeRideCalculator from '../components/HomeRideCalculator.vue'; // @ is an alias to /src

@Options({
  components: {
    HomeRideCalculator,
  },
})
export default class HomeView extends Vue {}
</script>

<style lang="scss">
  .cal {
    margin: 5rem 0rem
  }
  
</style>
