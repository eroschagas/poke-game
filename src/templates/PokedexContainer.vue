<template>
  <div v-if="chosenPoke" class="pokedex-container">
    <img src="../assets/pokedex-pixel.png" alt="pokedex" />
    <div v-for="(item, index) in clickedPokemon" :key="index">
      <transition name="pokedexOnOff">
        <div v-if="!!clickedPokemon.length && index == this.page">
          <PokedexScreenName :fontResize="fontResize" :name="item.name" />
          <PokedexScreenImage
            :fontResize="fontResize"
            :src="pokeSprite(item.id)"
            :alt="item.name"
          />
          <PokedexStats type="type" :pokemon="item" :typeSprite="typeSprite" />
          <PokedexStats
            :fontResize="fontResize"
            :fontResizeIcon="fontResizeIcon"
            type="stat"
            name="gen"
            :stat="item.pokemon_v2_pokemonspecy.generation_id"
            :chosen="chosenPoke.pokemon_v2_pokemonspecy.generation_id"
          />
          <PokedexStats
            :fontResize="fontResize"
            :fontResizeIcon="fontResizeIcon"
            type="stat"
            name="weight"
            :stat="item.weight"
            :chosen="chosenPoke.weight"
          />
          <PokedexStats
            :fontResize="fontResize"
            :fontResizeIcon="fontResizeIcon"
            type="stat"
            name="height"
            :stat="item.height"
            :chosen="chosenPoke.height"
          />
        </div>
      </transition>
    </div>
    <div @click="nextPage" class="controller right">
      <ion-icon :style="fontResize" name="caret-forward-outline"></ion-icon>
    </div>
    <div @click="prevPage" class="controller left">
      <ion-icon :style="fontResize" name="caret-back-outline"></ion-icon>
    </div>
    <div class="controller top">
      <ion-icon :style="fontResize" name="caret-up-outline"></ion-icon>
    </div>
    <div class="controller bottom">
      <ion-icon :style="fontResize" name="caret-down-outline"></ion-icon>
    </div>
  </div>
</template>

<script>
// import PokeStat from '@/components/PokeStat.vue';
import PokedexScreenName from '@/components/PokedexScreenName.vue';
import PokedexScreenImage from '../components/PokedexScreenImage.vue';
import PokedexStats from '../components/PokedexStats.vue';
/* eslint-disable */
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
  computed: {
    fontResize() {
      if (window.screen.width < 600) {
        return { 'font-size': '2.1vw' };
      }
      return { 'font-size': '11px' };
    },
    fontResizeIcon() {
      if (window.screen.width < 600) {
        return { 'font-size': '3.6vw' };
      }
      return { 'font-size': '20px' };
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
  max-width: 100%;
  height: auto;
  margin: 10px;
  position: relative;
  img {
    max-width: 500px;
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
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  // background: red;
  width: 5.4%;
  height: 6.4%;
  cursor: pointer;
  color: white;
  p {
    font-size: 12px;
  }
}
.right {
  top: 78.9%;
  left: 39.8%;
}
.top {
  top: 73.9%;
  left: 35.4%;
  // background-color: white;
}
.left {
  top: 78.9%;
  left: 31.1%;
}
.bottom {
  top: 84%;
  left: 35.4%;
}
</style>
