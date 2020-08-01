<template>
  <v-card class="game mx-auto">
    <v-img height="200px" :src="`data:image/jpeg;base64,${game.image}`">
    </v-img>
    <v-card-title>{{ game.name }}</v-card-title>

    <v-card-actions>
      <v-btn text @click="sendGame">
        Send
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: "Game",
  props: {
    game: { type: Object, default: undefined },
    gameIndex: { type: Number, default: undefined }
  },
  methods: {
    sendGame() {
      this.$emit("sendGame", this.game.file);
    }
  }
};
</script>

<style scoped lang="scss">
.game {
  margin: 0.5em;
  width: 300px;
}
</style>
