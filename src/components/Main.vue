<template>
  <div class="content">
    <div class="jumbotron">
      <div class="jumbotron__head shadow">
        <h1 class="display-4 text-center">Fast Typing</h1>
        <p class="lead text-center">This is a simple fast typing game which created by Vue.js</p>
      </div>
      <hr class="my-4">
      <p>You have to press "space" each time to step next word</p>
      <div class="count__block d-flex ">
          <h4>total : {{this.trueCount+this.falseCount}}</h4>
          <h4>true : {{this.trueCount}}</h4>
          <h4>false : {{this.falseCount}}</h4>
      </div>
      <div class="card">
        <div v-if="this.isFinish" class="card-body text-center finished__block">
          <h2 class="pb-5">Game is finished</h2>
          <button style="padding : 10px 80px; font-size: 22px; line-height:22px" @click="reset__game" class="btn btn-primary btn-lg shadow">Reset</button>
        </div>
      </div>
      <div v-if="!this.isFinish" class="card__content">
        <div class="card">
          <div class="card-body">
            <span class="word__span" v-for="(word,key) in words.filter((data,index)=>index<20)"
              :class="key==0 ? writeControl : null" :key="key">{{word}}</span>
          </div>
        </div>
        <div class="card mt-4">
          <div class="card-body">
            <div class="input-group">
              <input type="text" class="form-control" aria-describedby="basic-addon2" v-model="write__words">
              <div class="input-group-append" style="margin-left:5px">
                <button class="btn btn-outline-secondary shadow" disabled type="button">{{timerCount}}s</button>
                <button class="btn btn-outline-secondary shadow" @click="getWords" type="button">Reset</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
  import words from '../assets/words.json'
  export default {

    data() {
      return {
        words: [],
        write__words: null,
        isTrue: true,
        trueCount: 0,
        falseCount: 0,
        timerCount: 60,
        interval: false,
        timerStart: false,
        isFinish: false,
        wordList: words.commonWords
      }
    },
    watch: {
      write__words(value) {
        if (!value || value === ' ') {
          this.write__words = ''
          return
        }
        if (!this.timerStart) {
          this.timer()
        }
        const word = this.words[0].slice(0, value.length)
        const entered__word = value.replace(' ', '');
        this.isTrue = word === entered__word
        if (value.indexOf(' ') !== -1) {
          this.isTrue ? this.trueCount++ : this.falseCount++
          this.words.splice(0, 1)
          this.write__words = ''
          this.isTrue = true
        }
      }
    },
    computed: {
      writeControl() {
        return this.isTrue ? 'word__span__first' : 'word__span__first bg-danger'
      }
    },
    methods: {
      reset__game() {
        this.getWords()
        this.isFinish = false
        this.timerCount = 60;
        this.isTrue = true
        this.timerStart = false
        this.write__words = '';
        this.falseCount = 0;
        this.trueCount = 0;
      },
      timer() {
        this.timerStart = true
        this.interval = setInterval(() => {
          this.timerCount--
          if (this.timerCount === 0) {
            clearInterval(this.interval)
            this.isFinish = true
            return
          }
        }, 1000)
        this.timerStart = true
      },
      getWords() {
        this.words = this.wordList.sort(() => Math.random() - 0.5).splice(0, 300);
      }
    },
    mounted() {
      this.getWords()
    }
  }

</script>

<style scoped>
  .word__span {
    margin-left: 10px;
    font-size: 30px;
    font-weight: 400;
  }

  .word__span__first {
    background-color: #14bdac;
    border-radius: 5px;
    color: #fff;
    padding: 0 20px;
  }

  .jumbotron__head {
    background-color: #14bdac;
    color: #fff;
    padding-bottom: 5px;
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;
  }
  .count__block h4{
    margin-right: 20px;
  }
  .finished__block{
    background-color: #14bdac;
    border-radius: 10px;
    color: #fff;
  }

</style>
