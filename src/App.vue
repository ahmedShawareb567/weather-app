<template>
  <div id="app">
    <weatherComponent />
  </div>
</template>
<script>
import weatherComponent from "./components/weather.vue";
export default {
  data() {
    return {};
  },
  components: {
    weatherComponent
  }
};
</script>
