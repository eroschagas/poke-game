<template>
  <div class="home">
    <!-- <div v-if="!mobile" class="menu">menu</div> -->
    <div class="console" @click="teste">console.log</div>
    <PokeApiError :pokeApi="pokeApi" />
    <transition name="loading">
      <div v-if="Loading" class="loading"><LoadingIcon size="50" /></div>
    </transition>
    <transition class="guess-wrapper" name="loading">
      <div v-if="!Loading" :class="['container']">
        <PokeChosen
          :pokeList="pokeList"
          :randomPokemon="randomPokemon"
          :clickedPokemon="clickedPokemon"
          :pokeSprite="pokeSprite"
          :upperCase="upperCase"
          :typeSprite="typeSprite"
          :chosenPoke="chosenPoke"
          :pokeFound="pokeFound"
        />
        <PokedexContainer
          :clickedPokemon="clickedPokemonDelayed"
          :pokeSprite="pokeSprite"
          :chosenPoke="chosenPoke"
          :clickedPokemonLimit="clickedPokemonLimit"
          :typeSprite="typeSprite"
        />
      </div>
    </transition>
    <transition class="find-wrapper" name="loading">
      <div v-if="!Loading" class="list">
        <PokeList
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
    </transition>
  </div>
</template>

<script>
import axios from 'axios';
import PokeList from './templates/PokeList.vue';
import PokeChosen from './templates/PokeChosen.vue';
import PokeApiError from './components/PokeApiError.vue';
import LoadingIcon from './components/LoadingIcon.vue';
import PokedexContainer from './templates/PokedexContainer.vue';

export default {
  name: 'App',
  components: {
    PokeList,
    PokeChosen,
    PokeApiError,
    LoadingIcon,
    PokedexContainer,
  },
  data() {
    return {
      pokeList: [],
      clickedPokemonDelayed: [],
      clickedPokemonBuffer: [],
      clickedPokemonLimit: [],
      pokePush: false,
      pokeFound: false,
      Loading: true,
      allowLoadingFade: true,
      disableClick: 0,
      pokeApi: true,
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
  computed: {
    mobile() {
      return window.screen.width < 800 ? true : false;
    },
    clickedPokemon() {
      return this.pokeList.filter((p) => p.selected);
    },

    maxPokemon() {
      return this.pokeList.filter(
        (p) => JSON.parse(p.pokemon_v2_pokemonsprites[0].sprites).front_default
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
      console.log('chosenpoke', this.chosenPoke);
      console.log(this.pokeList);
      console.log(
        JSON.parse(
          this.pokeList[this.randomPokemon].pokemon_v2_pokemonsprites[0].sprites
        )
      );
    },

    async getPokemon() {
      this.Loading = true;
      this.allowLoadingFade = true;
      try {
        const response = await axios({
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
        this.pokeList = response.data.data.pokemon_v2_pokemon;
        console.log(response);
        setTimeout(() => {
          this.Loading = false;
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
            var imgPathOther = `https://raw.githubusercontent.com/PokeAPI${jsonOther.replace(
              '/media',
              '/sprites/master'
            )}`;

            return imgPathOther;

          case 'versions':
            var jsonVersions = JSON.parse(
              this.pokeList[id].pokemon_v2_pokemonsprites[0].sprites
            )[type][second_type][third_type].front_default;
            var imgPathVersions = `https://raw.githubusercontent.com/PokeAPI${jsonVersions.replace(
              '/media',
              '/sprites/master'
            )}`;

            return imgPathVersions;

          default:
            var jsonDefault = JSON.parse(
              this.pokeList[id - 1].pokemon_v2_pokemonsprites[0].sprites
            )[type];
            var imgPathDefault = `https://raw.githubusercontent.com/PokeAPI${jsonDefault.replace(
              '/media',
              '/sprites/master'
            )}`;
            return imgPathDefault;
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
  position: relative;
  display: flex;
  // flex-direction: column;
  height: 100vh;
}
.console {
  position: fixed;
  cursor: pointer;
}
.loading {
  position: fixed;
  z-index: 1000;
  width: 100vw;
  height: 100vh;
  background: white;
  display: flex;
  justify-content: center;
  align-items: center;
}
.loading-enter-active,
.loading-leave-active {
  transition: opacity 0.8s 1s ease;
}
.loading-enter-from,
.loading-leave-to {
  opacity: 0;
}
.container {
  // margin-top: 60px;
  display: flex;
  flex-direction: column;
  // width: 100%;
  flex-grow: 2;
  align-items: center;
  // justify-content: center;
  // position: fixed;
  position: sticky;
  z-index: 1000;
  background: white;
}
.mobile {
  flex-direction: column;
}
.menu {
  background: red;
  width: 100%;
  height: 60px;
  position: fixed;
  z-index: 1000;
}
// .list {
//   margin-top: 500px;
// }
.guess-wrapper {
  position: sticky;
  flex-grow: 2;
  width: 700px;
}
.find-wrapper {
  flex-grow: 1;
  width: 100%;
  height: 100%;
  overflow: auto;
}
</style>
