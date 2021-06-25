<template>
  <div id="app">
    <app-form></app-form>
  </div>
</template>

<script>
import Form from "./components/AppForm.vue";

export default {
  name: "App",
  components: {
    "app-form": Form,
  },
};
</script>

<style>
</style>
