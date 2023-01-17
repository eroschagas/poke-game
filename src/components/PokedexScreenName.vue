<template>
  <div class="pokedex-screen-name">
    <h2>
      {{ name }}
    </h2>
  </div>
</template>

<script>
export default {
  name: 'PokedexScreenName',
  props: {
    name: String,
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
  overflow: hidden;
  h2 {
    font-size: 13px;
    text-align: center;
    font-family: $pixel;
  }
}
</style>
