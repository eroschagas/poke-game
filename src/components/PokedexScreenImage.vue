<template>
  <div @click="nextPage" class="pokedex-screen-image">
    <div>
      <img :src="src" :alt="alt" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokedexScreenImage',
  props: {
    src: String,
    alt: String,
  },
};
</script>

<style lang="scss" scoped>
@import '../variables.scss';
.pokedex-screen-image {
  height: 16%;
  width: 25.3%;
  position: absolute;
  left: 12.8%;
  top: 47.3%;
  overflow: hidden;
  animation: screen 0.5s infinite linear, glow 0.5s infinite linear;
  div {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    // background: red;
    img {
      height: 150%;
      width: auto;
      // background: green;
      image-rendering: pixelated;
    }
  }
}
</style>
