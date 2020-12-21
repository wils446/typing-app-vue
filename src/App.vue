<template>
  <div id="app">
    <div class="container">
      <h3 class="display-3 text-center mb-4">Typing Game</h3>
      <h4 class="typing-content" v-html="wordsToShow">{{wordsToType}}</h4>
      
      <div class="input-group my-5">
        <input type="text" class="form-control" v-model="typeInput" @keyup="addInput" @keydown.enter="start"
          placeholder="Type Here...">

        <div class="input-group-append">
          <span v-if="isPlaying" class="input-group-text" id="basic-addon2">{{time}}</span>
          <span v-else class="input-group-text" id="basic-addon2">Time</span>
        </div>
      </div>

      <button v-on:click="start" class="btn btn-success">Start</button><br>
      <span >(or press enter)</span><br>
      <span>permainan akan selesai jika benar semua</span>
      <h4 class="mt-3">Score : {{score.score}} (Characters per minutes)</h4>
    </div>
  </div>
</template>

<script>
import { indonesianWords } from "./words";

export default {
  name: 'App',
  data() {
    return {
      isPlaying: false,
      wordsToType: "",
      typeInput: "", //v-model
      time: 0,
      timer: 0,
      score: {
        time : [],
        score : 0,
        scoreAverage : 0
      }
    }
  },
  methods: {
    start() {
      if (this.isPlaying) return;
      this.isPlaying = true;
      this.time = 0;
      this.typeInput = "";

      // Generate words
      const words = [];
      for (let x = 0; x < 20; x++) {
        const random = Math.floor(Math.random() * indonesianWords.length);
        words.push(indonesianWords[random]);
      }
      this.wordsToType = words.join(" ");

      // Start timer
      this.timer = setInterval(() => {
        this.time += 1;
      }, 1000);

    },
    addInput() {
      // If finished
      if(this.typeInput.length === 0 || this.wordsToType.length === 0)return;
      if (this.typeInput === this.wordsToType) {
        this.isPlaying = false;
        //const reducer = (accumulator, currentValue) => accumulator + currentValue;
        clearInterval(this.timer);
        this.score.time.push(this.time);
        let operationScore = this.wordsToType.length/(this.time/60);
        this.score.score = operationScore.toFixed(1);
        //this.score.scoreAverage = ((this.score.score.reduce(reducer)/this.score.score.length-1).toFixed(1));  
        this.isPlaying = false;
        this.wordsToType = "";
        this.typeInput = "";
      }
    },
  },
  computed: {
    wordsToShow() {
      let html = ""

      for (let i = 0; i < this.wordsToType.length; i++) {
        if (this.typeInput[i] === undefined) html += this.wordsToType[i];
        else {
          html += `<span style="color:white; background-color: ${this.typeInput[i] === this.wordsToType[i] ? "green" : "red"};">${this.wordsToType[i]}</span>`
        }
      }
      return html;
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
    margin-top: 20px;
  }

  .typing-content {
    justify-content: center;
  }
</style>