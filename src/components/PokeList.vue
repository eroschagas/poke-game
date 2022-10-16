<template>
  <div @click="teste">console.log</div>
  <div class="all-pokemon">
    <div v-for="(item, index) in pokeList" :key="index">
      <div
        @click="pickPokemon(item.id)"
        :class="[
          'poke-card',
          pokeMissed(item) ? 'poke-card-disabled' : '',
          pokeFound(item) ? 'poke-card-found' : '',
        ]"
        v-if="pokeList.length && item.is_default && pokeSprite(index)"
      >
        <!-- <h1>{{ item.name }}</h1> -->
        <img :src="pokeSprite(index)" :alt="item.name" />
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
    'pokeFound',
    'pokeMissed',
  ],
  data() {
    return {};
  },
  methods: {
    teste() {
      console.log(this.clickedPokemon);
      console.log(this.pokeList);
      console.log(
        JSON.parse(this.pokeList[0].pokemon_v2_pokemonsprites[0].sprites)
      );
    },
  },
};
</script>

<style lang="sass">
@import '../variables.sass'
.all-pokemon
  display: flex
  flex-wrap: wrap
  margin: 10px
  justify-content: center

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
  background: #ff1e2d

.poke-card-disabled
  background: #727272

.poke-card-disabled:hover
  background: #727272
  transform: scale(1.0)

.poke-card-found
  position: relative
  z-index: 10
  background: yellow
  transform: scale(1.5)

.poke-card-found:hover
  background: yellow
  transform: scale(1.5)
</style>
