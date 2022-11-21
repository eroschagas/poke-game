<template>
  <div class="poke-list">
    <div class="poke-search">
      <input
        @keyup.enter="pokePick(pokeListFilter[0])"
        ref="inputSearch"
        type="text"
        v-model="filter"
      />
    </div>
    <div class="list-wrapper">
      <div :key="testekey" class="all-pokemon">
        <div
          :class="allowLoadingFade ? 'poke-cards' : ''"
          v-for="(item, index) in pokeListFilter"
          :key="index"
        >
          <div v-if="pokeFound" class="win-overlay"></div>
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
            <!-- <h1>{{ item.name }}</h1> -->
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
      </div>
    </div>
    <div class="all-pokemon pokelist-loading" v-if="loadingList">
      <LoadingIcon speed="2" size="30" />
    </div>
  </div>
</template>

<script>
import LoadingIcon from '../components/LoadingIcon.vue';
export default {
  name: 'PokeList',
  props: [
    'pokeList',
    'pickPokemon',
    'pokeSprite',
    'chosenPoke',
    'pokeFind',
    'pokeFound',
    'pokeMissed',
    'loadingList',
    'allowLoadingFade',
  ],
  components: { LoadingIcon },
  data() {
    return {
      filter: '',
      testekey: 0,
    };
  },
  watch: {
    filter() {
      this.testekey += 1;
    },
  },
  computed: {
    pokeListFilter() {
      if (this.filter.length > 0) {
        return this.pokeList.filter((p) => p.name.indexOf(this.filter) > -1);
      }
      return this.pokeList;
    },
  },
  methods: {
    pokePick(item) {
      this.pickPokemon(item);
      setTimeout(() => {
        this.filter = '';
        this.$refs.inputSearch.focus();
      }, 500);
    },
  },
};
</script>

<style lang="sass">
@import '../variables.sass'

@keyframes openPokeballTop
  0%
    translate: 0
  100%
    translate: 0 -10px

@keyframes openPokeballBot
  0%
    translate: 0
  100%
    translate: 0 10px

.all-pokemon
  margin: 0 10px
  max-height: 100%
  padding: 25px
  overflow-y: scroll
  display: flex
  flex-wrap: wrap
  justify-content: flex-start
  align-items: flex-start

.poke-cards
  opacity: 0
  transition: all 1s ease-out
  animation: fadeIn 0.5s 2s forwards ease-out

.win-overlay
  width: 100%
  height: 100%
  position: fixed
  z-index: 100
  background: $transparent

.poke-list
  padding-top: 20px
  // height: calc( 100vh - 30px )
  width: 100%
  // margin: 0 10px
  // flex-grow: 3


.poke-card
  width: 90px
  height: 90px
  // background: $pokelistBackground
  // background-image: url('../assets/Poke_Ball_icon_top.svg')
  border-radius: 50%
  margin: 5px
  display: flex
  justify-content: center
  align-items: center
  transition: all 0.1s ease-in-out
  cursor: pointer
  // box-shadow: 0px 0px 2px 2px $shadow
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
  div
    width: 90px
    height: 90px
    display: flex
    justify-content: center
    align-items: center
    position: relative
    img
      image-rendering: pixelated
      position: relative
      border-radius: 50%
      background-image: radial-gradient( $pokeBackground 30%, transparent 70%)
      width: 70px
      height: 70px
.poke-card-hover:hover
  // background: $pokelistBackgroundHover
  scale: (1.25)
  div
    opacity: 1
    animation-name: shake, grow-shrink
    animation-duration: 0.4s, 0.5s
    animation-timing-function: ease-in-out, ease-in-out
    animation-iteration-count: 1, 1

.poke-card-disabled
  img
    transition: all 0.5s ease-in-out
    filter: brightness(0)
  .pokeball-top
    animation: openPokeballTop 0.3s forwards ease-out
  .pokeball-bot
    animation: openPokeballBot 0.3s forwards ease-out
  &:hover
    .pokeball-top
      translate: 0 -10px
    .pokeball-bot
      translate: 0 10px

.poke-card-found
  position: relative
  z-index: 10
  background: $pokelistBackgroundWin
  transform: scale(1.3)

.poke-card-found:hover
  background: $pokelistBackgroundWin
  transform: scale(1.3)

.pokelist-loading
  position: absolute
  top: 0
  width: 100%
  height: 100%
  overflow: hidden
  display: flex
  justify-content: center
  align-items: center
  background: $shadow

.list-loading
  height: 50px
  width: 50px

.poke-search
  height: 30px
  margin: 10px 70px
  input
    height: 30px
    width: 300px
    outline: none
.list-wrapper
  height: calc( 100vh - 70px )
  position: relative
  &::before
    content: ''
    background: $fadeColorBottom
    // background: red
    position: absolute
    width: calc( 100% - 30px )
    height: 20px
    top: 0
    left: 0
    z-index: 10
  &::after
    content: ''
    background: $fadeColorTop
    position: absolute
    width: calc( 100% - 30px )
    height: 20px
    bottom: 0
    left: 0
    z-index: 10
</style>
