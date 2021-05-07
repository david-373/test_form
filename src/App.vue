<template>
  <div id="app">
    <FormValidation />
  </div>
</template>

<script>
import FormValidation from "./components/FormValidation";
export default {
  name: "App",
  components: {
    FormValidation,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
