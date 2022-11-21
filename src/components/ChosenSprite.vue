<template>
  <div class="chosen-sprite">
    <div class="chosen-title">
      <div class="chosen-name">
        <ChosenName :name="upperCase(chosenPoke.name)" :pokeFound="pokeFound" />
      </div>
      <div
        class="chosen-image"
        :style="`background-image: url(${require(`../assets/background/${backgroundPick()}_bg.png`)})`"
      >
        <div
          v-if="pokeFound"
          class="pokechosen-open pokechosen-appear-effect"
        ></div>
        <div
          v-if="pokeFound"
          class="pokechosen-open pokechosen-sprite-mask"
          :style="maskImage"
        ></div>
        <!-- <img
          v-if="true"
          class="pokechosen-sprite"
          :src="pokeSprite(randomPokemon)"
          alt="Secret Pokemon"
          :style="bottomCalc(chosenPoke.height)"
        /> -->
        <img
          v-if="pokeFound"
          class="pokechosen-sprite"
          :src="
            pokeSprite(randomPokemon, 'versions', 'generation-viii', 'icons')
          "
          alt="Secret Pokemon"
        />
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
import ChosenName from './ChosenName.vue';

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
      random: Math.floor(Math.random() * 8),
    };
  },
  created() {
    this.backgroundPick();
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
    maskImage() {
      return `-webkit-mask-image: url(${this.pokeSprite(
        this.randomPokemon,
        'versions',
        'generation-viii',
        'icons'
      )})`;
    },
    tile() {
      const tiles = [
        'forest',
        'grass',
        'ground',
        'rock',
        'snow',
        'street',
        'water',
        'wood',
      ];
      return require(`../assets/typeTile/${tiles[this.random]}-tile.png`);
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
    backgroundPick() {
      const background = [
        'forest',
        'grass',
        'desert',
        'cave',
        'snow',
        'dark',
        'water',
        'mistery',
      ];
      return background[this.random];
    },

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
  components: { ChosenName },
};
</script>

<style lang="sass">
@import '../variables.sass'

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
    position: absolute
    z-index: 100
    scale: 4
    height: 56px
    width: 68px
    bottom: 100px
    image-rendering: pixelated
    animation-name: fadeInLightPoke, pokeJump
    animation-duration: 2s, 0.8s
    animation-timing-function: linear, linear
    animation-iteration-count: 1, 1
    animation-delay: 0s, 5s
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
  // background-image: url('../assets/background/basic.png')
  background-repeat: no-repeat
  background-size: cover
  image-rendering: pixelated
  overflow: hidden
  &::after
    content: ''
    width: 30px
    height: 100%
    background: $fadeColorLeft
    position: absolute
    right: 0
    z-index: 100
  &::before
    content: ''
    width: 30px
    height: 100%
    background: $fadeColorRight
    position: absolute
    left: 0
    z-index: 100

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

.pokechosen-open
  background: red
  position: absolute

.pokechosen-appear-effect
  z-index: 102
  bottom: -55px
  width: 230px
  height: 230px
  border-radius: 50%
  opacity: 0
  scale: 0
  animation-name: lightOut, lightForm
  animation-duration: 1.2s, 2.5s
  animation-timing-function: ease-out, ease-in-out
  animation-iteration-count: 1, 1
  animation-delay: .3s, 1.5s

.pokechosen-sprite-mask
  height: 56px
  width: 68px
  scale: 4
  opacity: 0
  bottom: 100px
  z-index: 101
  mask-mode: alpha
  mask-repeat: no-repeat
  -webkit-mask-mode: alpha
  -webkit-mask-repeat: no-repeat
  image-rendering: pixelated
  animation-name: fadeInLight, fadeOut
  animation-duration: 3s, .8s
  animation-timing-function: linear, ease-out
  animation-iteration-count: 1, 1
  animation-delay: 1s, 4s
</style>
