<script>
import Vue from 'vue';
import { ArsenalSample } from '@/entry';

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
