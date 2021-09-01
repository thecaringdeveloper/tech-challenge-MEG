<template>
  <Layout :game-details="gameDetails"
          :right-guesses="rightGuesses"
          :next-card="nextCard"
          :rightly-guessed-cards="rightlyGuessedCards"
          @higher="higher"
          @lower="lower"
          @restart="init"/>
</template>

<script>

import Layout from "@/components/Layout";

const cards = [
  "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A",
]

export default {
  name: 'Game',
  components: {Layout},
  data: () => {
    return {
      gameDetails: {
        won: 0,
        lost: 0,
        deck: []
      },
      rightGuesses: 0,
      rightlyGuessedCards: []
    }
  },
  created() {
    this.init()
  },
  computed: {
    nextCard() {
      return this.gameDetails.deck[this.gameDetails.deck.length - 1]
    }
  },
  watch: {
    rightGuesses(newValue) {
      if (newValue > 4) {
        this.won()
      }
    }
  },
  methods: {
    init() {
      let gameDeck = []
      let sourceDeck = [...cards]
      for (let i = 0; i < 52; i++) {
        if (sourceDeck.length === 0) {
          sourceDeck = [...cards]
        }
        const selectedCard = Math.floor(Math.random() * (sourceDeck.length))
        gameDeck.push(sourceDeck[selectedCard])
        sourceDeck.splice(selectedCard, 1)
      }
      this.gameDetails.deck = gameDeck
      this.rightGuesses = 0
      this.rightlyGuessedCards = []
    },
    won() {
      this.gameDetails.won++
      this.init()
    },
    lost() {
      this.gameDetails.lost++
    },
    continueGame() {
      const topCard = this.nextCard
      this.gameDetails.deck.splice(this.gameDetails.deck.length - 1, 1)
      const nextCard = this.nextCard
      return {
        currentCardLabel: topCard,
        currentCardValue: cards.indexOf(topCard),
        nextCardValue: cards.indexOf(nextCard)
      }
    },
    higher() {
      const cards = this.continueGame()
      if (cards.nextCardValue > cards.currentCardValue) {
        this.rightGuesses++
        this.rightlyGuessedCards.push(cards.currentCardLabel)
      } else {
        this.lost()
      }
    },
    lower() {
      const cards = this.continueGame()
      if (cards.nextCardValue < cards.currentCardValue) {
        this.rightGuesses++
        this.rightlyGuessedCards.push(cards.currentCardLabel)
      } else {
        this.lost()
      }
    }
  }
}
</script>
