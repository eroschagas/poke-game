<template>
  <div class="home">
    <PokeApiError :pokeApi="pokeApi" />
    <transition name="loading">
      <div v-if="loadingList" class="loading"><LoadingIcon size="50" /></div>
    </transition>
    <div class="main-wrapper" v-if="pokeApi">
      <PokePick
        :typeSprite="typeSprite"
        :clickedPokemon="clickedPokemonDelayed"
        :pokeSprite="pokeSprite"
        :chosenPoke="chosenPoke"
        :clickedPokemonLimit="clickedPokemonLimit"
      />

      <div class="section-guess">
        <!-- <div class="console" @click="teste">console.log</div> -->
        <div ref="sectionGuess">
          <PokeChosen
            :loadingList="loadingList"
            :pokeList="pokeList"
            :randomPokemon="randomPokemon"
            :clickedPokemon="clickedPokemon"
            :pokeSprite="pokeSprite"
            :upperCase="upperCase"
            :typeSprite="typeSprite"
            :chosenPoke="chosenPoke"
            :pokeFound="pokeFound"
          />
        </div>
        <PokeList
          :sectionGuessHeight="sectionGuessHeight"
          :loadingList="loadingList"
          :pokeList="pokeList"
          :pickPokemon="pickPokemon"
          :pokeSprite="pokeSprite"
          :chosenPoke="chosenPoke"
          :pokeFind="pokeFind"
          :pokeFound="pokeFound"
          :pokeMissed="pokeMissed"
          :allowLoadingFade="allowLoadingFade"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokeList from './templates/PokeList.vue';
import PokeChosen from './templates/PokeChosen.vue';
import PokePick from './templates/PokePick.vue';
import PokeApiError from './components/PokeApiError.vue';
import LoadingIcon from './components/LoadingIcon.vue';

export default {
  name: 'App',
  components: {
    PokeList,
    PokeChosen,
    PokePick,
    PokeApiError,
    LoadingIcon,
  },
  data() {
    return {
      pokeList: [],
      clickedPokemonDelayed: [],
      clickedPokemonBuffer: [],
      clickedPokemonLimit: [],
      pokePush: false,
      pokeFound: false,
      loadingList: true,
      allowLoadingFade: true,
      disableClick: 0,
      pokeApi: true,
      sectionGuessHeight: 0,
    };
  },
  async created() {
    await this.getPokemon();
  },
  watch: {
    pokePush() {
      if (this.clickedPokemonBuffer.length > 0 && this.pokePush == true) {
        this.buffer();
      }
    },
    clickedPokemon: {
      deep: true,
      handler() {
        if (this.clickedPokemon.includes(this.chosenPoke)) {
          this.pokeFound = true;
        } else {
          this.pokeFound = false;
        }
      },
    },
  },
  updated() {
    this.sectionGuessHeight = this.$refs.sectionGuess.offsetHeight;
  },
  computed: {
    clickedPokemon() {
      return this.pokeList.filter((p) => p.selected);
    },

    maxPokemon() {
      return this.pokeList.filter(
        (p) => JSON.parse(p.pokemon_v2_pokemonsprites[0].sprites).front_default
        // JSON.parse(p.pokemon_v2_pokemonsprites[0].sprites).other.dream_world
        //   .front_default
      );
    },
    randomPokemon() {
      return Math.floor(Math.random() * this.maxPokemon.length);
      // return 5;
    },
    chosenPoke() {
      if (this.pokeList) {
        return this.pokeList[this.randomPokemon];
      }
      return {};
    },
  },
  methods: {
    buffer() {
      this.clickedPokemonDelayed.push(this.clickedPokemonBuffer[0]);
      this.clickedPokemonBuffer.shift();
      setTimeout(() => {
        if (this.clickedPokemonBuffer.length == 0) {
          this.pokePush = false;
          return;
        }
        this.buffer();
      }, 750);
    },
    teste() {
      console.log(this.clickedPokemon);
      console.log(this.pokeList);
      console.log(
        JSON.parse(
          this.pokeList[this.randomPokemon].pokemon_v2_pokemonsprites[0].sprites
        )
      );
      console.log(this.sectionGuessHeight);
    },

    async getPokemon() {
      this.loadingList = true;
      this.allowLoadingFade = true;
      try {
        const result = await axios({
          method: 'POST',
          url: 'https://beta.pokeapi.co/graphql/v1beta',
          data: {
            // eslint-disable-next-line
            query: `{ pokemon_v2_pokemon(where: {is_default: {_eq: true}, pokemon_v2_pokemonsprites: {sprites: {_lt: "{\\\"front_default\\\": null"}}}, order_by: {id: asc}) {
                  id
                  name
                  height
                  weight
                  pokemon_v2_pokemontypes {
                    slot
                    pokemon_v2_type {
                      name
                    }
                  }
                  pokemon_v2_pokemonspecy {
                    generation_id
                  }
                  pokemon_v2_pokemonsprites {
                    sprites
                  }
                }
              }`,
          },
        });
        this.pokeList = result.data.data.pokemon_v2_pokemon;
        setTimeout(() => {
          this.loadingList = false;
        }, 2000);
        setTimeout(() => {
          this.allowLoadingFade = false;
        }, 3000);
        this.pokeApi = true;
      } catch (error) {
        console.error(error);
        this.pokeApi = false;
      }
    },
    typeSprite(type) {
      return require('./assets/typeSprite/' + this.upperCase(type) + '.svg');
    },
    pokeSprite(id = 0, type = 'front_default', second_type, third_type) {
      // let k = this.pokeList.find((p) => p.id == id);
      // let k = this.pokeList[id + 1];
      if (this.pokeList.length) {
        switch (type) {
          case 'other':
            var jsonOther = JSON.parse(
              this.pokeList[id].pokemon_v2_pokemonsprites[0].sprites
            )[type][second_type].front_default;
            return jsonOther;

          case 'versions':
            var jsonVersions = JSON.parse(
              this.pokeList[id].pokemon_v2_pokemonsprites[0].sprites
            )[type][second_type][third_type].front_default;
            return jsonVersions;

          default:
            var jsonDefault = JSON.parse(
              this.pokeList[id - 1].pokemon_v2_pokemonsprites[0].sprites
            )[type];
            return jsonDefault;
        }
      }
    },
    upperCase(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    pickPokemon(item) {
      if (this.clickedPokemon.includes(item)) {
        return;
      }
      // if (Date.now() - this.disableClick < 700) {
      //   return;
      // }
      // this.disableClick = Date.now();

      if (!this.pokeMissed(item) && !this.pokeFound) {
        item.selected = true;
        this.clickedPokemonBuffer.push(item);
        if (this.pokePush == false) {
          this.pokePush = true;
        }
      }
    },
    pokeFind(pick) {
      if (this.chosenPoke == pick && this.clickedPokemon.includes(pick)) {
        return true;
      }
      return false;
    },
    pokeMissed(pick) {
      if (this.chosenPoke != pick && this.clickedPokemon.includes(pick)) {
        return true;
      }
      return false;
    },
  },
};
</script>

<style lang="scss" scoped>
.home {
  display: flex;
}
.console {
  position: fixed;
  cursor: pointer;
}
.section-guess {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: fit-content;
  width: 100%;
}
.loading {
  position: fixed;
  z-index: 10000000;
  width: 100vw;
  height: 100vh;
  background: white;
  display: flex;
  justify-content: center;
  align-items: center;
}
.loading-leave-active {
  transition: opacity 0.8s 1s ease;
}
.loading-leave-to {
  opacity: 0;
}
.main-wrapper {
  display: flex;
  width: 100%;
  // height: 100%
}
</style>
