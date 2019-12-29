<template>
  <div id="app">
    <Header
      :latestScore="childData"
      :questionLength="questions.length"
      :done="done.length"
      @finalScore="finalScore"
    />
    <b-container>
      <b-row>
        <b-col sm="6" lg="12" class="my-4">
          <QuestionBox 
            v-if="questions.length"
              :questionLength="questions.length"
              :currentQuestion="questions[index]"
              :next="next"
              :index="index"
              :done="done"
              :finalScores="finalScores"
              @correct="updateScore"
          />          
        </b-col>
      </b-row>
  </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import _ from 'lodash'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: null,
      done: [],
      childData: 0,
      finalScores: 0
    }
  },
  methods: {
    next() {
      let temp = this.index
      let breaking = 0
      if(this.done.length != this.questions.length) {
        while(breaking==0) {
          this.index = _.random(0,this.questions.length-1)
          if((this.done.find(element => element == this.index) == null) && (this.index != temp)) {
            breaking = 1
          }
        }
      }
    },
    updateScore(score) {
      this.childData = score
    },
    finalScore(final) {
      this.finalScores = final
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=31&difficulty=medium', {
      method: 'get'
    })
      .then((response) => {
        return response.json()
      })
      .then((jsonData) => {
        this.questions = jsonData.results
        this.index = Math.ceil(Math.random()*(Object.keys(this.questions).length-1))
      })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
  font-size: 1.1em;
}
</style>
