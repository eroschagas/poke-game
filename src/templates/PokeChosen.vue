<template>
  <div v-if="chosenPoke && !loadingList" class="chosen-box">
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
        :chosen="true"
        :poke="chosenPoke"
        :clickedPokemon="clickedPokemon"
        :typeSprite="typeSprite"
      />
      <PokeStat
        statName="GEN"
        :chosen="true"
        :found="findGen()"
        :stat="chosenPoke.pokemon_v2_pokemonspecy.generation_id"
      />
      <PokeStat
        statName="WEI"
        :chosen="true"
        :found="findWeigth()"
        :stat="chosenPoke.weight / 10"
        unit="kg"
      />
      <PokeStat
        statName="HEI"
        :chosen="true"
        :found="findHeight()"
        :stat="chosenPoke.height / 10"
        unit="m"
      />
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
import PokeStat from '@/components/PokeStat.vue';

export default {
  name: 'PokeChosen',
  components: { LoadingIcon, TypeImage, ChosenSprite, PokeStat },
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
  width: 315px
  border-radius: 25px
  background: $transparent
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center

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
