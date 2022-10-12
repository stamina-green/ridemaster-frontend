<template>
  <div class="about">
    <h1>This is an about page</h1>
    <HomeRideCalculator>

    </HomeRideCalculator>
  </div>
</template>
<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import HomeRideCalculator from '../components/HomeRideCalculator.vue';

@Options({
  components: {
    HomeRideCalculator,
  },
})
export default class HomeView extends Vue {}
</script>