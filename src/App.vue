<template>
  <div id="app">
    <Wheel v-if="isSpinning" :spinTo="result" :values="values" />
    <div id="app__choices">
      <div v-for="i in choices" :key="i" v-show="!isSpinning"><input v-model="values[i-1]" type="text" /></div>
    </div>
    <button v-show="!isSpinning" @click="choices++">Add one more</button>
    <button v-show="!isSpinning" @click="spin">Spin that wheel !</button>
    <button v-show="isSpinning" @click="spin">Do it again !</button>
    <span>By <a href="http://leiluspocus.netlify.app">Laïla</a></span>
  </div>
</template>

<script>
import Wheel from './components/Wheel.vue'; 

export default {
  name: 'App',
  data() {
    return {
      isSpinning: false,
      choices : 3,
      values: [],
      result: ''
    }
  },
  components: {
    Wheel
  },
  methods: {
    spin() {
      this.isSpinning = true;
      this.result = this.values[Math.floor(Math.random() * this.values.length)];
    },
    selectResult(msg) {
      console.log(msg);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
