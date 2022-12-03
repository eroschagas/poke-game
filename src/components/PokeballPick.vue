<template>
  <div
    :class="allowLoadingFade ? 'poke-cards' : ''"
    v-for="(item, index) in pokeListFilter"
    :key="index"
  >
    <div v-if="pokeFound" class="win-overlay"></div>
    <!-- <h1>{{ item.name }}</h1> -->

    <div
      @click="pokePick(item)"
      :class="[
        'poke-card',
        'fade-in',
        pokeMissed(item) ? 'poke-card-disabled' : 'poke-card-hover',
        pokeFind(item) ? 'poke-card-found' : '',
      ]"
      v-if="pokeList.length"
    >
      <div>
        <div class="pokeball-top"></div>
        <div class="pokeball-bot"></div>
        <img
          :class="(allowLoadingFade ? 'fade-in-delay' : '', 'hide-alt')"
          :src="pokeSprite(item.id)"
          :alt="item.name"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokeballPick',
  props: [
    'allowLoadingFade',
    'pokeListFilter',
    'pokeSprite',
    'pokeMissed',
    'pokeFind',
    'pokePick',
    'pokeList',
    'pokeFound',
  ],
};
</script>

<style lang="sass" scoped>
@import '../variables.sass'

@keyframes openPokeballTop
  0%
    translate: 0
  100%
    translate: 0 -5px

@keyframes openPokeballBot
  0%
    translate: 0
  100%
    translate: 0 5px


.poke-card
  width: 90px
  height: 90px
  // background: $pokelistBackground
  // background-image: url('../assets/Poke_Ball_icon_top.svg')
  border-radius: 50%
  margin: 7px
  display: flex
  justify-content: center
  align-items: center
  transition: all 0.1s ease-in-out
  cursor: pointer
  position: relative
  // box-shadow: 0px 0px 2px 2px $shadow
  div
    width: 90px
    height: 90px
    display: flex
    justify-content: center
    align-items: center
    img
      image-rendering: pixelated
      // position: relative
      border-radius: 50%
      background-image: radial-gradient( $pokeBackground 30%, transparent 70%)
      width: 70px
      height: 70px
      z-index: 100
.poke-card-hover:hover
  // background: $pokelistBackgroundHover
  scale: (1.2)
  div
    opacity: 1
    animation-name: shake, grow-shrink
    animation-duration: 0.4s, 0.5s
    animation-timing-function: ease-in-out, ease-in-out
    animation-iteration-count: 1, 1


.poke-card-found
  position: relative
  z-index: 10
  background: $pokelistBackgroundWin
  transform: scale(1.3)

.poke-card-found:hover
  background: $pokelistBackgroundWin
  transform: scale(1.3)

.pokeball-top
    width: 90px
    height: 90px
    position: absolute
    top: 0
    left: 0
    background-image: url('../assets/Poke_Ball_icon_top.svg')
.pokeball-bot
    width: 90px
    height: 90px
    position: absolute
    top: 0
    left: 0
    background-image: url('../assets/Poke_Ball_icon_bot.svg')
.poke-card-disabled
  img
    transition: all 0.5s ease-in-out
    filter: brightness(0)
  .pokeball-top
    animation: openPokeballTop 0.3s forwards ease-out
  .pokeball-bot
    animation: openPokeballBot 0.3s forwards ease-out
  .pokeball-top
    translate: 0 -10px
  .pokeball-bot
    translate: 0 10px
</style>
