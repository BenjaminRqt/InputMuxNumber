<template>
  <div id="app">
    <InputMuxNumber
      :min=0
      :max=10000
      name="test-input-max"
      label="Test input mux"
      suffix="€"
      step="25"
    />
  </div>
</template>

<script>
import InputMuxNumber from './components/InputMuxNumber.vue';

export default {
  name: 'App',
  components: {
    InputMuxNumber,
  },
};
</script>

<style>
#app {
  text-align: center;
  margin-top: 60px;
}
</style>
