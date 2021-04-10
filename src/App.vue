<template>
  <div id="app">
    <TrafficLight />
  </div>
</template>

<script>
  import TrafficLight from "./components/TrafficLight";

  export default {
    components: {
      TrafficLight
    },
    // mounted() {
    //   console.log(window.location.href)
    //   console.log('mounted');
    //   history.pushState(null, 'Красный', '/red');
    // },
    // methods: {
    //   test() {
    //     history.pushState(null, 'Красный', '/green');
    //   }
    // }
  }
</script>
