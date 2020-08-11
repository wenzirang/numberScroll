<template>
  <div id="app">
    <number-scroll :isSelfIncreasing="true" :number="650560" :font-size="48" :suffix="'+'"></number-scroll>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import NumberScroll from './components/number-scroll.vue'

@Component({
  components: {
    NumberScroll,
  },
})
export default class App extends Vue {}
</script>
<style>
* {
  padding: 0;
  margin: 0;
}
</style>