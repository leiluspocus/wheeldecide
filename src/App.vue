<template>
  <div id="app">
    <Wheel v-if="isSpinning" :spinTo="result" :values="values" />
    <div id="app__choices">
      <div v-for="i in choices" :key="i" v-show="!isSpinning">
        <input v-model="values[i-1]" type="text" />
      </div>
      <span id="plus" v-show="!isSpinning" @click="choices++">➕</span>
    </div>
    <div id="app_ctas">
      <button v-show="!isSpinning" @click="spin">Spin that wheel !</button>
      <button v-show="isSpinning" @click="revert">Do it again !</button>
    </div>
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
    revert() {
      this.isSpinning = false;
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

#plus:hover {
  cursor: pointer;
}

#app__choices {
  display: flex;
  flex-direction: column;
  align-items: center;
}

#app_ctas {
  margin-top: 30px;
} 
</style>
