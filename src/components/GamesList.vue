<template>
  <div class="games-list">
    <div class="games-list__form">
      <h2>Favorite genre: {{showFavoriteGenre}}</h2>
      <h2>Game with highest score: {{highestRatedFavorite}}</h2>
      <!-- <h2>{{this.$store.state.totalScores}}</h2> -->
      <label for="name" class="games-list__label">Name of the game</label>
      <input v-model="newGame.name" class="games-list__input" id="name" />
      <label for="game_genre" class="games-list__label">Select game's genre</label>
      <select v-model="newGame.genre" class="games-list__select" id="game_genre" name="genres">
        <option value disabled selected>Select genre...</option>
        <option value="Real Time Strategy">Real Time Strategy</option>
        <option value="Role Playing Game">Role Playing Game</option>
        <option value="Turn Based Strategy">Turn Based Strategy</option>
        <option value="First Person Shooter">First Person Shooter</option>
        <option value="Action & Adventure">Action & adventure</option>
        <option value="Sports & Racing">Sports & Racing</option>
      </select>
      <label for="release_date" class="games-list__label">Release date</label>
      <input v-model="newGame.release" class="games-list__input" id="release_date" />
      <label for="developer" class="games-list__label">Developer's name</label>
      <input v-model="newGame.developer" class="games-list__input" id="developer" />
      <label for="game_image" class="games-list__label">Upload an image</label>
      <input
        type="file"
        accept="image/*"
        @change="uploadImage($event)"
        class="games-list__input"
        id="game_image"
        ref="fileInput"
      />
      <button @click="addGame" class="games-list__button">Add game</button>
    </div>
    <EachGame v-for="item in games" :item="item" :key="item.name" />
  </div>
</template>

<script>
import EachGame from "./EachGame.vue";
export default {
  name: "GamesList",
  components: {
    EachGame
  },
  data() {
    return {
      newGame: {
        name: null,
        genre: null,
        release: null,
        developer: null,
        image: null
      },
      games: [
        {
          name: "Metro: Exodus",
          genre: "First Person Shooter",
          release: 2019,
          developer: "4A Games",
          image: "images/metroExodus.jpg"
        },
        {
          name: "Dishonored 2",
          genre: "Action & Adventure",
          release: 2016,
          developer: "Arkane Studios",
          image: "images/dishonored2.jpg"
        },
        {
          name: "Half-Life 2",
          genre: "First Person Shooter",
          release: 2004,
          developer: "Valve",
          image: "images/halfLife2.jpg"
        }
      ]
    };
  },

  computed: {
    isDuplicate() {
      return this.games.some(item => {
        return item.name.toLowerCase() === this.newGame.name.toLowerCase();
      });
    },
    showFavoriteGenre() {
      const counts = {};
      this.games.map(item => {
        counts[item.genre] = (counts[item.genre] || 0) + 1;
      });
      let favoriteGameGenre = Object.keys(counts).reduce((a, b) =>
        counts[a] > counts[b] ? a : b
      );
      return favoriteGameGenre;
    },
    highestRatedFavorite() {
      if (this.$store.state.totalScores.length != 0) {
        let highestRated = this.$store.state.totalScores.reduce((a, b) =>
          a.score > b.score ? a : b
        );
        return highestRated.name;
      } else {
        return "You haven't rated a game yet";
      }
    }
  },
  methods: {
    addGame() {
      if (
        this.newGame.name != null &&
        this.newGame.genre != null &&
        this.newGame.release != null &&
        this.newGame.developer != null
      ) {
        if (!this.isDuplicate) {
          this.games.push(this.newGame);
          this.newGame = {};
          this.$refs.fileInput.value = "";
        } else {
          alert("There is already a game with that name in the library");
        }
      } else {
        alert("Please fill all the inputs");
      }
    },
    uploadImage(e) {
      const image = e.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(image);
      reader.onload = e => {
        this.newGame.image = e.target.result;
      };
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.games-list {
  margin: auto;
  text-align: center;
}
.games-list__form {
  border-bottom: 3px #3369ff solid;
  display: flex;
  flex-direction: column;
  margin: auto;
  max-width: 400px;
  text-align: left;
  padding: 20px 0;
}
.games-list__input {
  height: 20px;
}
.games-list__select {
  height: 26px;
}
.games-list__label {
  padding: 12px 0 6px;
}
.games-list__button {
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