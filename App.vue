<script>
import ChartComponent from './components/ChartComponent.vue';

export default {
  name: 'App',
  components: {
    ChartComponent,
  },
};
</script>

<template>
  <div class="chart">
    <ChartComponent />
  </div>
</template>

<style>
.chart {
  display: flex;
  align-items: center;
}
</style>
