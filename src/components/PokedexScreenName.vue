<template>
  <div ref="screenName" class="pokedex-screen-name">
    <h2 :style="fontResize">{{ name }}</h2>
  </div>
</template>

<script>
export default {
  name: 'PokedexScreenName',
  props: {
    name: String,
    fontResize: Object,
  },
};
</script>

<style lang="scss" scoped>
@import '../variables.scss';
.pokedex-screen-name {
  height: 3.6%;
  width: 33.5%;
  animation: screen 0.5s infinite linear, glow 0.5s infinite linear;
  // box-shadow: 0px 0px 5px 2px #028539;
  color: white;
  position: absolute;
  left: 8.6%;
  top: 32%;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  h2 {
    text-align: center;
    font-family: $pixel;
  }
}
</style>
