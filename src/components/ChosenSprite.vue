<template>
  <div class="chosen-sprite">
    <div class="chosen-title">
      <div class="chosen-name">
        <h1 v-if="pokeFound">{{ upperCase(chosenPoke.name) }}</h1>
        <h1 v-else>Who is that pokémon?</h1>
      </div>
      <div class="chosen-image">
        <img
          v-if="false"
          class="pokechosen-sprite"
          :src="pokeSprite(this.randomPokemon)"
          alt="Secret Pokemon"
        />
        <!-- <img
          v-if="pokeFound"
          class="pokechosen-sprite"
          :src="pokeSprite(this.randomPokemon, 'other', 'official-artwork')"
          alt="Secret Pokemon"
        /> -->
        <div
          class="chosen-pokeball chosen-pokeball-closed"
          v-show="open == 'closed'"
        >
          <img :src="pokeball('closed')" alt="pokeball" />
          <div class="red-dot"></div>
        </div>
        <div v-show="open == 'opening'" class="chosen-pokeball">
          <img :src="pokeball('opening')" alt="pokeball-opening" />
        </div>
        <div
          v-show="open == 'opened'"
          class="chosen-pokeball chosen-pokeball-opened"
        >
          <img :src="pokeball('opened')" alt="pokeball-opened" />
        </div>
        <div class="chosen-tile">
          <img :src="tile" alt="" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ChosenSprite',
  props: [
    'upperCase',
    'pokeSprite',
    'randomPokemon',
    'chosenPoke',
    'pokeFound',
  ],
  data() {
    return {
      open: 'closed',
    };
  },
  watch: {
    pokeFound() {
      if (!this.pokeFound) {
        this.open = 'closed';
      }
      if (this.pokeFound) {
        this.open = 'opening';
        setTimeout(() => {
          this.open = 'opened';
        }, 300);
      }
    },
  },
  computed: {
    tile() {
      const tiles = [
        'forest-tile',
        'grass-tile',
        'ground-tile',
        'rock-tile',
        'snow-tile',
        'street-tile',
        'water-tile',
        'wood-tile',
      ];
      let random = Math.floor(Math.random() * 8);
      return require(`../assets/typeTile/${tiles[random]}.png`);
    },
    pokeballType() {
      const pokeball = ['pokeball', 'greatball', 'ultraball', 'masterball'];
      const random = Math.floor(Math.random() * 99) + 1;
      if (random < 40) {
        return pokeball[0];
      }
      if (random < 70) {
        return pokeball[1];
      }
      if (random < 90) {
        return pokeball[2];
      }
      return pokeball[3];
    },
  },
  methods: {
    pokeball(status) {
      switch (status) {
        case 'closed':
          return require(`../assets/pokeballs/${this.pokeballType}.png`);
        case 'opening':
          return require(`../assets/pokeballs/${this.pokeballType}-opening.png`);
        case 'opened':
          return require(`../assets/pokeballs/${this.pokeballType}-opened.png`);
      }
    },
  },
};
</script>

<style lang="sass">
@import '../variables.sass'

@keyframes redDotGlow
  0%
    opacity: 0
  50%
    opacity: 0
  70%
    opacity: 1
  90%
    opacity: 0
  100%
    opacity: 0



@keyframes rotatePokeball
  0%
    rotate: 0deg
  10%
    rotate: -25deg
  30%
    rotate: 25deg
  40%
    rotate: 0deg
  100%
    rotate: 0deg

.chosen-title
  display: flex
  flex-direction: column
  align-items: center
  h1
    margin: 5px
    font-family: $pixel
    font-weight: normal
    text-shadow: 2px 2px 2px $shadow
  .pokechosen-sprite
    // position: relative
    // position: absolute
    z-index: 100
    // scale: 1.5
    // margin-bottom: 10px
    image-rendering: pixelated
    // max-height: 60%
    // transform-box: fill-box
    // transform-origin: 50% 100%
  .chosen-pokeball
    image-rendering: pixelated
    z-index: 50
    position: absolute
    bottom: 10px

.chosen-image
  display: flex
  justify-content: center
  height: 200px
  width: 300px
  position: relative

.chosen-tile
  display: flex
  justify-content: center
  width: 300px
  position: absolute
  bottom: 0
  img
    image-rendering: pixelated
    width: auto
    z-index: 0

.chosen-pokeball-closed
  transform-origin: 50% 70%
  animation: rotatePokeball 2s 1s infinite ease-in-out

.chosen-pokeball-opened
  animation: fadeOut 3s 2s forwards ease-out

.red-dot
  opacity: 0
  height: 8px
  width: 8px
  background: $redglow
  position: absolute
  box-shadow: 0 0 2px 1px $redglow
  top: 48px
  left: 49px
  animation: redDotGlow 2s 1s infinite ease-in-out

.chosen-name
  display: flex
  justify-content: center
  align-items: center
  text-align: center
  height: 82px
</style>
