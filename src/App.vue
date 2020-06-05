<script>
import Gauge from './components/Gauge'

export default {
  name: 'App',
  components: {
    Gauge
  },
  data() {
    return {
      finishOffset: 50,
      maximumValue: 240,
      minimumValue: 0,
      startOffset: 50,
      value: 0,
    };
  },
}
</script>

<template>
  <div id="app">
    <div id="gauge-container">
      <Gauge
          id="gauge"
          v-bind:finish-offset="finishOffset"
          v-bind:min="minimumValue"
          v-bind:max="maximumValue"
          v-bind:start-offset="startOffset"
          v-bind:value="value"
      />
    </div>
    <input
        id="value"
        type="range"
        v-model="value"
        v-bind:min="minimumValue"
        v-bind:max="maximumValue"
    />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 100%;
  height: 100%;
}

#gauge-container {
  padding-bottom: 20px;
  width: 600px;
  margin-left: auto;
  margin-right: auto;
}

#gauge {
  width: 600px;
  height: 600px;
}

#value {
  width: 600px;
  justify-self: center;
}
</style>
