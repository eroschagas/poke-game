<template>
  <div class="poke-list" :style="calculatedHeight">
    <div class="poke-search">
      <input
        @keyup.enter="pokePick(pokeListFilter[0])"
        ref="inputSearch"
        type="text"
        v-model="filter"
      />
    </div>
    <div class="list-wrapper">
      <div class="all-pokemon">
        <PokeballPick
          :allowLoadingFade="allowLoadingFade"
          :pokeListFilter="pokeListFilter"
          :pokeSprite="pokeSprite"
          :pokeFind="pokeFind"
          :pokeMissed="pokeMissed"
          :pokeList="pokeList"
          :pokePick="pokePick"
          :pokeFound="pokeFound"
        />
      </div>
    </div>
  </div>
  <div class="all-pokemon pokelist-loading" v-if="loadingList">
    <LoadingIcon speed="2" size="30" />
  </div>
</template>

<script>
import PokeballPick from '@/components/PokeballPick.vue';
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
    'sectionGuessHeight',
  ],
  components: { LoadingIcon, PokeballPick },
  data() {
    return {
      filter: '',
    };
  },
  computed: {
    calculatedHeight() {
      return `height: calc(100vh - ${this.sectionGuessHeight}px)`;
    },
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

.poke-list
  padding-top: 20px
  width: 100%
  display: flex
  flex-direction: column
  // margin: 0 10px
  // flex-grow: 3
.all-pokemon
  // margin: 0 10px
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
  width: 100%
  height: 30px
  // margin: 10px 70px
  input
    height: 30px
    width: 300px
    outline: none
.list-wrapper
  // height: calc( 100vh - 70px )

  max-height: calc( 100% - 30px )
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
