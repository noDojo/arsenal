<script>
import Vue from 'vue';
import { ArsenalSample } from '@/main';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    ArsenalSample,
  }
});
</script>

<template>
  <div id="app">
    <arsenal-sample />
  </div>
</template>
