<template>
  <div v-if="chosenPoke" class="chosen-box">
    <!-- <div v-if="pokeFound" class="chosen-title"> -->
    <!-- <div v-if="true" class="chosen-title">
      <h1>{{ upperCase(chosenPoke.name) }}</h1>
      <img
        :src="pokeSprite(this.randomPokemon, 'other', 'official-artwork')"
        alt="Secret Pokemon"
      />
    </div> -->
    <ChosenSprite
      class="chosen-sprite"
      :upperCase="upperCase"
      :pokeSprite="pokeSprite"
      :randomPokemon="randomPokemon"
      :chosenPoke="chosenPoke"
      :pokeFound="pokeFound"
    />

    <div class="chosen-stats">
      <TypeImage
        class="chosen-stats-field"
        chosen="true"
        :poke="chosenPoke"
        :clickedPokemon="clickedPokemon"
        :typeSprite="typeSprite"
      />
      <div class="chosen-stats-field chosen-gen">
        <div>
          <p>GEN</p>
        </div>
        <div v-if="findGen()" class="chosen-gen-number fadeIn-grow">
          <p>
            {{ chosenPoke.pokemon_v2_pokemonspecy.generation_id }}
          </p>
        </div>
        <div v-else class="chosen-number"></div>
      </div>
      <div class="chosen-stats-field chosen-weight">
        <div>
          <p>WEIGHT</p>
        </div>
        <div v-if="findWeigth()" class="chosen-weight-number fadeIn-grow">
          <p>{{ chosenPoke.weight / 10 }}kg</p>
        </div>
        <div v-else class="chosen-number"></div>
      </div>
      <div class="chosen-stats-field chosen-height">
        <div>
          <p>HEIGHT</p>
        </div>
        <div v-if="findHeight()" class="chosen-height-number fadeIn-grow">
          <p>{{ chosenPoke.height / 10 }}m</p>
        </div>
        <div v-else class="chosen-number"></div>
      </div>
    </div>
  </div>
  <div class="chosen-box pokechosen-loading" v-else>
    <LoadingIcon speed="2" size="50" />
  </div>
</template>

<script>
import TypeImage from '@/components/TypeImage.vue';
import LoadingIcon from '@/components/LoadingIcon.vue';
import ChosenSprite from '@/components/ChosenSprite.vue';

export default {
  name: 'PokeChosen',
  components: { LoadingIcon, TypeImage, ChosenSprite },
  props: [
    'pokeList',
    'clickedPokemon',
    'randomPokemon',
    'pokeSprite',
    'upperCase',
    'typeSprite',
    'chosenPoke',
    'pokeFound',
    'loadingList',
  ],
  data() {
    return {};
  },
  methods: {
    findGen() {
      let clicked = this.clickedPokemon.map(
        (p) => p.pokemon_v2_pokemonspecy.generation_id
      );
      let chosen = this.chosenPoke.pokemon_v2_pokemonspecy.generation_id;
      if (clicked.includes(chosen)) {
        return true;
      } else {
        return false;
      }
    },
    findWeigth() {
      let clicked = this.clickedPokemon.map((p) => p.weight);
      let chosen = this.chosenPoke.weight;
      if (clicked.includes(chosen)) {
        return true;
      } else {
        return false;
      }
    },
    findHeight() {
      let clicked = this.clickedPokemon.map((p) => p.height);
      let chosen = this.chosenPoke.height;
      if (clicked.includes(chosen)) {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style lang="sass">
@import '../variables.sass'
.chosen-box
  height: fit-content
  margin: 20px
  width: 450px
  border-radius: 25px
  background: $chosenBackground
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center
  box-shadow: 2px 2px 2px $shadow

.chosen-stats
  display: flex

.chosen-stats-field
  background: $chosenBackgroundFields
  display: flex
  justify-content: center
  align-items: center
  height: 50px
  box-shadow: 2px 2px 2px $shadow
  margin: 10px 5px
  padding: 0 10px
  white-space: nowrap
  border-radius: 10px
  font-family: $pixel
  font-weight: normal
  text-shadow: 2px 2px 2px $shadow

.chosen-gen
  width: 75px
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center

.chosen-weight
  width: 85px
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center


.chosen-height
  width: 85px
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center

.chosen-number
  height: 20px

.pokechosen-loading
  display: flex
  justify-content: center
  align-items: center

.chosen-loading
  height: 50px
  width: 50px
</style>
