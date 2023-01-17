<template>
  <div v-if="chosenPoke" class="pokedex-container">
    <img src="../assets/pokedex-pixel.png" alt="pokedex" />
    <div v-for="(item, index) in clickedPokemon" :key="index">
      <transition name="pokedexOnOff">
        <div v-if="!!clickedPokemon.length && index == this.page">
          <PokedexScreenName :name="item.name" />
          <PokedexScreenImage
            @click="nextPage"
            :src="pokeSprite(item.id)"
            :alt="item.name"
          />
          <PokedexStats type="type" :pokemon="item" :typeSprite="typeSprite" />
          <PokedexStats
            type="stat"
            name="gen"
            :stat="item.pokemon_v2_pokemonspecy.generation_id"
          />
          <PokedexStats
            type="stat"
            name="weight"
            :stat="!!clickedPokemon.length ? `${item.weight}kg` : ''"
          />
          <PokedexStats
            type="stat"
            name="height"
            :stat="!!clickedPokemon.length ? `${item.height}m` : ''"
          />
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
// import PokeStat from '@/components/PokeStat.vue';
import PokedexScreenName from '@/components/PokedexScreenName.vue';
import PokedexScreenImage from './PokedexScreenImage.vue';
import PokedexStats from './PokedexStats.vue';

export default {
  name: 'PokedexContainer',
  components: {
    PokedexScreenName,
    PokedexScreenImage,
    PokedexStats,
  },
  props: {
    clickedPokemon: Array,
    pokeSprite: Function,
    chosenPoke: Object,
    clickedPokemonLimit: Array,
    typeSprite: Function,
  },
  data() {
    return {
      key: 0,
      page: 0,
    };
  },
  watch: {
    clickedPokemon: {
      deep: true,
      handler() {
        console.log(this.key);
        this.key += 1;
      },
    },
  },
  methods: {
    nextPage() {
      this.page += 1;
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../variables.scss';
.pokedex-container {
  display: block;
  width: 600px;
  height: auto;
  margin: 10px;
  position: relative;
  img {
    max-width: 600px;
    width: 100%;
    height: auto;
  }
}
.pokedexOnOff-enter-active,
.pokedexOnOff-leave-active {
  transition: all 1s ease;
}
.pokedexOnOff-enter-from,
.pokedexOnOff-leave-to {
  opacity: 0;
}
</style>
