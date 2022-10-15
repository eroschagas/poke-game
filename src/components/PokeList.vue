<template>
  <div @click="teste">console.log</div>
  <div class="all-pokemon">
    <div v-for="(item, index) in pokeList" :key="index">
      <div
        @click="pickPokemon(index)"
        :class="[
          'poke-card',
          clickedPokemon.includes(index) ? 'poke-card-disabled' : '',
        ]"
        v-if="pokeList.length && item.is_default && pokeSprite(index)"
      >
        <!-- <h1>{{ item.name }}</h1> -->
        <img :src="pokeSprite(index)" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokeList',
  props: ['pokeList'],
  data() {
    return {
      clickedPokemon: [],
    };
  },
  computed: {
    maxPokemon() {
      return this.pokeList.filter(
        (p) =>
          p.is_default &&
          JSON.parse(p.pokemon_v2_pokemonsprites[0].sprites).front_default
      ).length;
    },
  },
  methods: {
    pokeSprite(index = 0) {
      if (this.pokeList.length) {
        const json = JSON.parse(
          this.pokeList[index].pokemon_v2_pokemonsprites[0].sprites
        ).front_default;
        return json;
      }
    },
    teste() {
      console.log(this.maxPokemon);
      console.log(this.clickedPokemon);
    },
    pickPokemon(index) {
      this.clickedPokemon.push(index);
    },
  },
};
</script>

<style lang="sass">
.all-pokemon
  display: flex
  flex-wrap: wrap
  margin: 10px
  justify-content: center

.poke-card
  width: 90px
  height: 90px
  background: #e4000f
  border-radius: 50%
  margin: 5px
  display: flex
  justify-content: center
  align-items: center
  transition: all 0.1s ease-in-out
  cursor: pointer
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
</style>
