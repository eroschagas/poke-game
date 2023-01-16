<template>
  <div v-if="chosenPoke" class="pokedex-container">
    <img src="../assets/pokedex-pixel.png" alt="pokedex" />
    <div class="pokedex-screen-name">
      <h2 v-if="!!clickedPokemon.length">
        {{ clickedPokemon[clickedPokemon.length - 1].name }}
      </h2>
    </div>
    <div class="pokedex-screen-image">
      <div>
        <img
          v-if="!!clickedPokemon.length"
          :src="pokeSprite(clickedPokemon[clickedPokemon.length - 1].id)"
          :alt="clickedPokemon[clickedPokemon.length - 1].name"
        />
      </div>
    </div>
    <div class="pokedex-stats">
      <transition name="pokedexType">
      <div v-if="!!clickedPokemon.length" class="pick-stats-container">
        <div :key="key" class="pick-stats">
            <TypeSprite
              :pokemon="clickedPokemon[clickedPokemon.length - 1]"
              :typeSprite="typeSprite"
            />
          </div>
        </div>
      </transition>
    </div>
    <div class="stat"><p>GEN</p></div>
    <div class="stat weight"><p>WEIGHT</p></div>
    <div class="stat height"><p>HEIGHT</p></div>
    <div class="stat value">
      <p>
        {{
          !!clickedPokemon.length
            ? clickedPokemon[clickedPokemon.length - 1].pokemon_v2_pokemonspecy
                .generation_id
            : ''
        }}
      </p>
    </div>
    <div class="stat value weight">
      <p>
        {{
          !!clickedPokemon.length
            ? `${clickedPokemon[clickedPokemon.length - 1].weight}kg`
            : ''
        }}
      </p>
    </div>
    <div class="stat value height">
      <p>
        {{
          !!clickedPokemon.length
            ? `${clickedPokemon[clickedPokemon.length - 1].height}m`
            : ''
        }}
      </p>
    </div>
  </div>
</template>

<script>
// import PokeStat from '@/components/PokeStat.vue';
import TypeSprite from '@/components/TypeSprite.vue';

export default {
  name: 'PokedexContainer',
  components: { TypeSprite },
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
.pokedex-screen-name {
  height: 3.6%;
  width: 33.5%;
  background: repeating-linear-gradient(#028539, #26a872 2px, #028539 5px);
  color: white;
  position: absolute;
  left: 8.6%;
  top: 32%;
  overflow: hidden;
  h2 {
    font-size: 13px;
    text-align: center;
    font-family: $pixel;
  }
}
.pokedex-screen-image {
  height: 16%;
  width: 25.3%;
  background: repeating-linear-gradient(#028539, #26a872 2px, #028539 5px);
  position: absolute;
  left: 12.8%;
  top: 47.3%;
  overflow: hidden;
  div {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    // background: red;
    img {
      height: 150%;
      width: auto;
      // background: green;
      image-rendering: pixelated;
    }
  }
}
.pokedex-stats {
  height: 8.5%;
  width: 34.9%;
  background: repeating-linear-gradient(#028539, #26a872 2px, #028539 5px);
  position: absolute;
  left: 58%;
  top: 46%;
}

.pick-stats-container {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  // background: green;
  // scale: 0.8;
  // transform-origin: top;
}
.pick-stats {
  display: flex;
  width: fit-content;
  height: 100%;
  width: 50%;
  justify-content: center;
  transform-origin: top;
  // background: blue;
}
.stat {
  width: 10.4%;
  height: 5%;
  background: repeating-linear-gradient(#028539, #26a872 2px, #028539 5px);
  color: white;
  position: absolute;
  top: 61%;
  left: 58.1%;
  display: flex;
  align-items: center;
  justify-content: center;
  p {
    font-size: 12px;
    text-align: center;
    font-family: $pixel;
  }
}
.weight {
  top: 72.5%;
}
.height {
  top: 84%;
}
.value {
  left: 76.8%;
  width: 15.8%;
}
.pokedexType-enter-active,
.pokedexType-leave-active {
  transition: all 1s ease;
}
.pokedexType-enter-from,
.pokedexType-leave-to {
  opacity: 0;
}
</style>
