<template>
  <VApp>
    <VAppBar app color="primary" dark>
      <VToolbarTitle>GoNetBoot</VToolbarTitle>
      <v-spacer></v-spacer>
      <VBtn icon @click="reloadGames">
        <VIcon>mdi-reload</VIcon>
      </VBtn>
    </VAppBar>

    <VMain>
      <GameList>
        <Game
          v-for="(game, index) in games"
          :key="`wnb-game-${index}`"
          :game="game"
          :game-index="index"
          @sendGame="sendGame"
        />
      </GameList>
    </VMain>
    <VOverlay :value="displayOverlay">
      <VProgressCircular indeterminate size="64" />
    </VOverlay>
    <VDialog v-model="displayLoadingOverlay" persistent>
      <VCard color="primary" dark class="app__booting">
        <VCardText>
          Please wait, booting {{ gameBooting }}
          <VProgressLinear
            indeterminate
            color="white"
            class="app__booting-bar mb-0"
          ></VProgressLinear>
        </VCardText>
      </VCard>
    </VDialog>
  </VApp>
</template>

<script>
import Game from "./components/Game";
import GameList from "./components/GameList";
const axios = require("axios").default;
export default {
  name: "App",

  components: { GameList, Game },
  data() {
    return {
      games: undefined,
      displayLoadingOverlay: false,
      gameBooting: undefined
    };
  },
  computed: {
    displayOverlay() {
      return !this.games;
    }
  },
  mounted() {
    this.fetchGames();
  },
  methods: {
    async fetchGames() {
      this.games = undefined;
      const {data} = await axios.get("http://localhost:8080/games");
      this.games = data;
    },
    async reloadGames() {
      this.games = undefined;
      const {data} = await axios.get("http://localhost:8080/reload");
      this.games = data;
    },
    async sendGame(file) {
      this.displayLoadingOverlay = true;
      this.gameBooting = file;
      const {data} = await axios.get("http://localhost:8080/sendGame", {
        params: { game: file }
      });
      console.log(data);

    }
  }
};
</script>
<style scoped lang="scss">
.app {
  &__booting {
    padding: 2em 1em;
  }
  &__booting-bar {
    margin-top: 0.5em;
  }
}
</style>
