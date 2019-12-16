<template>
  <div class="game">
    <img :src="item.image" class="game__image" />
    <div v-show="!detailsState">
      <h2>{{item.name}}</h2>
      <button @click="toggleDetails" class="games__button">View details</button>
    </div>
    <div v-show="detailsState" class="game__details">
      <h2>{{item.name}}</h2>
      <p>Genre: {{item.genre}}</p>
      <p>Developed by: {{item.developer}}</p>
      <p>Release date: {{item.release}}</p>
      <a target="_blank" :href="'https://www.amazon.de/s?k=' + item.name">Buy on Amazon</a>
      <button
        @click="rateGame"
        class="games__button"
      >{{calculatedTotalScore != '0' ? 'Edit rating' : 'Rate game'}}</button>
      <button @click="toggleDetails" class="games__button">Back</button>
      <div v-show="toggleScore">
        <p>Graphics: {{graphicsScore}}/20</p>
        <p>Sound: {{soundScore}}/15</p>
        <p>Gameplay: {{gameplayScore}}/30</p>
        <p>Feeling: {{feelingScore}}/20</p>
        <p>Storyline: {{storylineScore}}/5</p>
        <p>General impression: {{impressionScore}}/10</p>
        <h3>Total score</h3>
        <p>{{calculatedTotalScore}}</p>
      </div>
    </div>
    <div v-show="toggleRate">
      <h3>Rate the game</h3>
      <p>Graphics</p>
      <input type="number" v-model="graphicsScore" name="graphics__score" min="0" max="20" />
      <p>Sound</p>
      <input type="number" v-model="soundScore" name="sound__score" min="0" max="15" />
      <p>Gameplay</p>
      <input type="number" v-model="gameplayScore" name="gameplay__score" min="0" max="30" />
      <p>Feeling</p>
      <input type="number" v-model="feelingScore" name="feeling__score" min="0" max="20" />
      <p>Storyline</p>
      <input type="number" v-model="storylineScore" name="storyline__score" min="0" max="5" />
      <p>General impression</p>
      <input type="number" v-model="impressionScore" name="impression__score" min="0" max="10" />
      <button @click="checkInputValues" class="games__button">Done</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "EachGame",
  props: ["item"],
  data() {
    return {
      toggleRate: false,
      detailsState: false,
      toggleScore: false,
      graphicsScore: 0,
      soundScore: 0,
      gameplayScore: 0,
      feelingScore: 0,
      storylineScore: 0,
      impressionScore: 0
    };
  },
  methods: {
    toggleDetails() {
      this.detailsState = !this.detailsState;
    },
    rateGame() {
      this.toggleRate = !this.toggleRate;
    },
    checkInputValues() {
      if (
        this.graphicsScore == 0 ||
        this.soundScore == 0 ||
        this.gameplayScore == 0 ||
        this.feelingScore == 0 ||
        this.storylineScore == 0 ||
        this.impressionScore == 0
      ) {
        alert("Please fill all the required inputs");
      } else {
        this.toggleScore = !this.toggleScore;
        this.toggleRate = !this.toggleRate;
        let obj = {};
        let isDuplicate = this.$store.state.totalScores.some(item => {
          return item.name == this.item.name;
        });
        if (isDuplicate == false) {
          obj["name"] = this.item.name;
          obj["genre"] = this.item.genre;
          obj["score"] = this.calculatedTotalScore;
          this.$store.state.totalScores.push(obj);
        } else {
          this.$store.state.totalScores.map((element, index) => {
            if (element.name === this.item.name) {
              this.$store.state.totalScores[
                index
              ].score = this.calculatedTotalScore;
            }
          });
        }
      }
    }
  },
  computed: {
    calculatedTotalScore() {
      const total =
        parseInt(this.graphicsScore) +
        parseInt(this.soundScore) +
        parseInt(this.gameplayScore) +
        parseInt(this.feelingScore) +
        parseInt(this.storylineScore) +
        parseInt(this.impressionScore);
      return total;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.game {
  padding-top: 10px;
}
.game__image {
  width: 145px;
  height: 200px;
}
.games__button {
  display: inline-block;
  padding: 0.7em 1.4em;
  margin: 0 0.3em 0.3em 0;
  border: none;
  border-radius: 0.15em;
  box-sizing: border-box;
  text-decoration: none;
  font-weight: 400;
  color: #ffffff;
  background-color: #3369ff;
  box-shadow: inset 0 -0.6em 0 -0.35em rgba(0, 0, 0, 0.17);
  text-align: center;
}
</style>
