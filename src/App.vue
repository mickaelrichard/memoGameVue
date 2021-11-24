<template>
  <div class="App">
    <h1>Magic Match</h1>
    <button @click="shuffleCards">New Game</button>

    <div class="card-grid">
      <SingleCard
        v-for="card in cards"
        :key="card.id"
        :card="card"
        @choice="handleChoice"
        :flipped="card === choiceOne || card === choiceTwo || card.matched"
        :disabled="disabled"
      />
    </div>

    <p>Turns: {{ turns }}</p>
  </div>
</template>

<script>
import SingleCard from "./components/SingleCard";

export default {
  name: "App",
  components: { SingleCard },
  data() {
    return {
      cardImages: [
        { src: "helmet-1.png", matched: false },
        { src: "potion-1.png", matched: false },
        { src: "ring-1.png", matched: false },
        { src: "scroll-1.png", matched: false },
        { src: "shield-1.png", matched: false },
        { src: "sword-1.png", matched: false },
      ],
      cards: [],
      turns: 0,
      choiceOne: null,
      choiceTwo: null,
      disabled: false,
    };
  },
  methods: {
    shuffleCards() {
      const shuffledCards = [...this.cardImages, ...this.cardImages]
        .sort(() => Math.random() - 0.5)
        .map((card) => ({ ...card, id: Math.random() }));

      this.choiceOne = null;
      this.choiceTwo = null;

      this.cards = shuffledCards;
      this.turns = 0;
      console.log(this.cards);
    },
    handleChoice(card) {
      if (this.choiceOne) {
        this.choiceTwo = card;
        console.log("one", this.choiceOne);
      } else {
        this.choiceOne = card;
        console.log("two", this.choiceTwo);
      }
    },
    resetTurn() {
      this.choiceOne = null;
      this.choiceTwo = null;
      this.turns = this.turns + 1;
      this.disabled = false;
    },
  },
  computed: {
    flipped(card) {
      if (card === this.choiceOne || card === this.choiceTwo || card.matched) {
        return true;
      }
    },
  },
  watch: {
    choiceOne(val) {
      console.log(val, "one");
      if (val && this.choiceTwo) {
        this.disabled = true;
        if (val.src === this.choiceTwo.src) {
          this.cards = this.cards.map((card) => {
            if (card.src === val.src) {
              return { ...card, matched: true };
            } else {
              return card;
            }
          });
          this.resetTurn();
        } else {
          setTimeout(() => this.resetTurn(), 1000);
        }
      }
    },
    choiceTwo(val) {
      console.log(val, "two");
      if (val && this.choiceOne) {
        this.disabled = true;
        if (val.src === this.choiceOne.src) {
          this.cards = this.cards.map((card) => {
            if (card.src === val.src) {
              return { ...card, matched: true };
            } else {
              return card;
            }
          });
          this.resetTurn();
        } else {
          setTimeout(() => this.resetTurn(), 1000);
        }
      }
    },
  },
  mounted() {
    this.shuffleCards();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Raleway:wght@100;200;300;400;500;600;700;800&display=swap");

/* base styles */
body {
  font-family: Raleway, sans-serif;
  margin: 0;
  font-size: 1.5em;
  text-align: center;
  background: #1b1523;
  color: #fff;
}
.App {
  max-width: 860px;
  margin: 40px auto;
}
button {
  background: none;
  border: 2px solid #fff;
  padding: 6px 12px;
  border-radius: 4px;
  color: #fff;
  font-weight: bold;
  cursor: pointer;
  font-size: 1em;
}
button:hover {
  background: #c23866;
  color: #fff;
}
.card-grid {
  margin-top: 40px;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-gap: 20px;
}
</style>
