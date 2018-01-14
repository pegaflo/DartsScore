<template>
  <div id="app">
    <h1>Dart Scoreboard</h1>

    <div class="scoreBoard">

      <span class="scoreLeft">{{scoreLeft}}</span>

      <div class="scores">
        <md-field>
          <label>First Dart</label>
          <md-input v-model="score[0]" placeholder="First Dart"></md-input>
        </md-field>

        <md-field>
          <label>Second Dart</label>
          <md-input v-model="score[1]" placeholder="Second Dart"></md-input>
        </md-field>

        <md-field>
          <label>Third Dart</label>
          <md-input v-model="score[2]" placeholder="Third Dart"></md-input>
        </md-field>

        <md-button class="md-raised md-primary" @click="submitScore()">Scored</md-button>

      </div>

      <div>
        <h2>Three Dart Average</h2>
        <span>{{threeDartAverage}}</span>
      </div>

      <div>
        <h2>Dart Average</h2>
        <span>{{dartAverage}}</span>
      </div>

      <div class="lastScores">
        <h2>Last Scores:</h2>
        <div v-for="score in lastScores">{{score}}</div>
      </div>
    </div>


    <md-button class="md-raised md-primary" @click="resetScore()">Reset to 501</md-button>
    <md-button class="md-raised md-primary" @click="randomFinish()">Random Finish</md-button>
  </div>
</template>

<script>
export default {
  name: 'DartScore',

  data: function () {
    return {
      lastScores: [],
      dartAverage: 0.0,
      threeDartAverage: 0.0,
      score: [null, null, null],
      result: 0,
      scoreLeft: 501
    }
  },
  methods: {
    resetScore: function () {
      this.scoreLeft = 501
    },
    randomFinish: function () {
      this.scoreLeft = Math.floor((Math.random() * 170) + 2)
    },
    submitScore: function () {
      let score1 = this.score[0].split(' ')
      let score2 = this.score[1].split(' ')
      let score3 = this.score[2].split(' ')

      this.result = 0
      this.threeDartAverage = 0.0
      this.dartAverage = 0.0


      if (score1.length === 2) {
        this.result += parseInt(score1[0]) * parseInt(score1[1])
      } else {
        this.result += parseInt(score1[0])
      }

      if (score2.length === 2) {
        this.result += parseInt(score2[0]) * parseInt(score2[1])
      } else {
        this.result += parseInt(score2[0])
      }

      if (score3.length === 2) {
        this.result += parseInt(score3[0]) * parseInt(score3[1])
      } else {
        this.result += parseInt(score3[0])
      }

      if (this.scoreLeft - this.result > 0) {
          this.scoreLeft -= this.result
          this.lastScores.push(this.result)
      }

      if ((this.scoreLeft - this.result === 0)) {
        console.log('Game won')
        this.scoreLeft -= this.result
      }

      for (var i=0; i<this.lastScores.length; i++) {
        this.threeDartAverage += this.lastScores[i]
        this.dartAverage += this.lastScores[i]
      }

      this.threeDartAverage = this.threeDartAverage / this.lastScores.length
      this.dartAverage = this.dartAverage / (this.lastScores.length * 3)
    }
  }
}
</script>

<style>
  .scoreBoard {
    width: 100%;
  }

  .scoreLeft {
    font-size: 36px;
  }

  .scores {
    display: flex;
    margin-left: 5%;
    justify-content: center;
  }

  .lastScores {
    span {
      width: 100%;
    }
  }
</style>
