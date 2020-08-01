<template>
  <div class="game-list d-flex flex-wrap">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "GameList"
};
</script>

<style scoped lang="scss">
.game-list {
  padding: 0.5em;
}
</style>
