<template>
  <div id="container">
    <div class="row">
      <Card :key="'card-'+card+index" v-for="(card, index) in rightlyGuessedCards" :value="card"/>
    </div>
    <div class="row" v-if="!showLostMessage && !showWonMessage">
      <div>
        <button @click="lower">Lower</button>
      </div>
      <div>
        <Card :value="nextCard"/>
      </div>
      <div>
        <button @click="higher">Higher</button>
      </div>
    </div>
    <div class="row">
      <h2>Statistics</h2>
    </div>
    <div class="row">
      <ul>
        <li>Won: {{gameDetails.won}}</li>
        <li>Lost: {{gameDetails.lost}}</li>
      </ul>
    </div>
    <div class="row">
      <p v-if="showLostMessage">You have lost the game :(</p>
      <p v-if="showWonMessage">You have won the game :)</p>
      <p v-if="showRightGuessMessage">You have guessed right! This is the {{getTimesText()}} time!</p>
    </div>
    <div class="row">
      <button @click="restart" v-if="showLostMessage || showWonMessage">Restart</button>
    </div>
  </div>
</template>

<script>
  import Card from "@/components/Card";

  export default{
    name: 'Layout',
    components: {Card},
    comments: [Card],
    props: {
      gameDetails: Object,
      rightGuesses: Number,
      nextCard: String,
      rightlyGuessedCards: Array
    },
    data: () => {
      return {
        showLostMessage: false,
        showWonMessage: false,
        showRightGuessMessage: false
      }
    },
    watch: {
      wins() {
        this.showRightGuessMessage = false;
        this.showWonMessage = true
      },
      losses() {
        this.showRightGuessMessage = false
        this.showLostMessage = true
      },
      rightGuesses() {
        if (this.showRightGuessMessage < 5 && this.showRightGuessMessage > 0) {
          this.showRightGuessMessage = true
        }
      }
    },
    computed: {
      wins() {
        return this.gameDetails.won
      },
      losses() {
        return this.gameDetails.lost
      }
    },
    methods: {
      higher() {
        this.$emit("higher")
      },
      lower() {
        this.$emit("lower")
      },
      restart() {
        this.showWonMessage = false;
        this.showLostMessage = false;
        this.showRightGuessMessage = false;
        this.$emit("restart")
      },
      getTimesText() {
        let text = ''
        switch (this.rightGuesses) {
          case 1: text = 'first'
            break
          case 2: text = 'second'
            break
          case 3: text = 'third'
            break
          case 4: text = 'fourth'
        }
        return text
      }
    }
  }
</script>

<style scoped>
  #container {
    display: flex;
    flex: 1;
    flex-direction: column;
    background-color: #dddddd;
    padding: 15px;
  }
  .row {
    display: flex;
    flex: 1;
    flex-direction: row;
    justify-content: space-evenly;
    align-items: center;
  }
</style>
