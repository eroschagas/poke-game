<template>
  <div v-if="chosenPoke" class="pokedex-container">
    <img src="../assets/pokedex-pixel.png" alt="pokedex" />
    <div v-for="(item, index) in clickedPokemon" :key="index">
      <transition name="pokedexOnOff">
        <div v-if="!!clickedPokemon.length && index == this.page">
          <PokedexScreenName :name="item.name" />
          <PokedexScreenImage :src="pokeSprite(item.id)" :alt="item.name" />
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
          <div @click="nextPage" class="controller right"></div>
          <div @click="prevPage" class="controller left"></div>
          <div class="controller top"></div>
          <div class="controller bottom"></div>
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
        this.$nextTick(() => {
          this.page = this.clickedPokemon.length - 1;
        });
      },
    },
  },
  methods: {
    nextPage() {
      if (this.page == this.clickedPokemon.length - 1) {
        this.page = 0;
      } else {
        this.page += 1;
      }
      console.log('page', this.page);
    },
    prevPage() {
      if (this.page == 0) {
        this.page = this.clickedPokemon.length - 1;
      } else {
        this.page -= 1;
      }
      console.log('page', this.page);
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
.controller {
  position: absolute;
  // background: red;
  width: 5.4%;
  height: 6.4%;
  cursor: pointer;
}
.right {
  top: 79%;
  left: 39.5%;
}
.top {
  top: 74%;
  left: 35.4%;
}
.left {
  top: 79%;
  left: 31.5%;
}
.bottom {
  top: 84%;
  left: 35.4%;
}
</style>
