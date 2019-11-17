<template>

        <app-metric></app-metric>

</template>

<script>
  import Metric from './components/Metric';

  export default {
    name: 'App',
    components: {
      appMetric: Metric,
    },
    data: () => ({
      //
    }),
  };
</script>
