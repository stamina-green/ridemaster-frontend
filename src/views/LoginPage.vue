<template>
  <div class="login">
    <LoginComponent></LoginComponent>
  </div>
</template>

<script lang="ts">
import LoginComponent from "@/components/LoginComponent.vue";
import { Options, Vue } from "vue-class-component";

@Options({
  components: {
    LoginComponent,
  },
})
export default class HomeView extends Vue {}
</script>

<style lang="scss"></style>
