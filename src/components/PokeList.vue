<template>
  <div class="all-pokemon">
    <div v-for="(item, index) in pokeList" :key="index">
      <div
        @click="pickPokemon(item)"
        :class="[
          'poke-card',
          pokeMissed(item) ? 'poke-card-disabled' : '',
          pokeFind(item) ? 'poke-card-found' : '',
        ]"
        v-if="pokeList.length && item.is_default && pokeSprite(index)"
      >
        <!-- <h1>{{ item.name }}</h1> -->
        <img
          :src="pokeSprite(index)"
          :alt="item.name"
          @load="onLoad(item.id)"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokeList',
  props: [
    'pokeList',
    'pickPokemon',
    'pokeSprite',
    'chosenPoke',
    'pokeFind',
    'pokeMissed',
  ],
  data() {
    return {
      isLoaded: [],
    };
  },
  methods: {
    onLoad(x) {
      console.log(x);
    },
  },
};
</script>

<style lang="sass">
@import '../variables.sass'
.all-pokemon
  display: flex
  flex-wrap: wrap
  margin: 0 10px
  justify-content: center
  height: 100vh
  padding-top: 10px
  overflow-y: scroll

.poke-card
  width: 90px
  height: 90px
  background: $pokelistBackground
  border-radius: 50%
  margin: 5px
  display: flex
  justify-content: center
  align-items: center
  transition: all 0.1s ease-in-out
  cursor: pointer
  box-shadow: 2px 2px 2px $shadow
  img
    width: 70px
    height: 70px

.poke-card:hover
  transform: scale(1.2)
  background: $pokelistBackgroundHover

.poke-card-disabled
  background: $pokelistBackgroundFail

.poke-card-disabled:hover
  background: $pokelistBackgroundFail
  transform: scale(1.0)

.poke-card-found
  position: relative
  z-index: 10
  background: $pokelistBackgroundWin
  transform: scale(1.5)

.poke-card-found:hover
  background: $pokelistBackgroundWin
  transform: scale(1.5)
</style>
