<template>
  <div class="poke-type">
    <div>
      <div
        :class="[
          chosen ? 'poke-type-show-chosen fadeIn-swirl-grow' : 'poke-type-show',
        ]"
        v-if="poke.pokemon_v2_pokemontypes && chosenType1"
      >
        <img
          :src="
            typeSprite(poke.pokemon_v2_pokemontypes[0].pokemon_v2_type.name)
          "
          :alt="
            chosen
              ? 'Secret Type'
              : poke.pokemon_v2_pokemontypes[0].pokemon_v2_type.name
          "
        />
      </div>
      <div :class="[chosen ? 'poke-type-hidden' : '']"></div>
    </div>
    <div>
      <div
        :class="[
          chosen ? 'poke-type-show-chosen fadeIn-swirl-grow' : 'poke-type-show',
        ]"
        v-if="
          poke.pokemon_v2_pokemontypes &&
          poke.pokemon_v2_pokemontypes.length == 2 &&
          chosenType2
        "
      >
        <img
          :src="
            typeSprite(poke.pokemon_v2_pokemontypes[1].pokemon_v2_type.name)
          "
          :alt="
            chosen
              ? 'Secret Type'
              : poke.pokemon_v2_pokemontypes[1].pokemon_v2_type.name
          "
        />
      </div>
      <div :class="[chosen ? 'poke-type-hidden' : '']"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TypeImage',
  props: ['poke', 'clickedPokemon', 'typeSprite', 'chosen'],
  data() {
    return {
      type1: false,
      type2: false,
    };
  },
  watch: {
    clickedPokemon: {
      deep: true,
      handler() {
        this.$nextTick(() => {
          this.findType();
        });
      },
    },
  },
  computed: {
    chosenType1() {
      if (this.chosen) {
        if (this.type1) {
          return true;
        }
        return false;
      }
      return true;
    },
    chosenType2() {
      if (this.chosen) {
        if (this.type2) {
          return true;
        }
        return false;
      }
      return true;
    },
  },
  methods: {
    findType() {
      let clicked = this.clickedPokemon
        .map((p) =>
          p.pokemon_v2_pokemontypes.map((p) => p.pokemon_v2_type.name)
        )
        .flat();
      let chosen = this.poke.pokemon_v2_pokemontypes.map(
        (p) => p.pokemon_v2_type.name
      );
      if (chosen.length > 1 && clicked.includes(chosen[1])) {
        this.type2 = true;
      }
      if (clicked.includes(chosen[0])) {
        this.type1 = true;
      }
    },
  },
};
</script>

<style lang="sass">
@import '../variables.sass'

.poke-type
  display: flex
  margin: 10px 5px
  align-items: center
  justify-content: center
  height: 50px
  width: 120px
  img
    height: 50px
    width: 50px

  .poke-type-show-chosen
    height: 50px
    width: 50px
    margin: 0 5px
    position: absolute
    img
      scale: 1.05
  .poke-type-show
    height: 50px
    width: 50px
    margin: 0 5px
  .poke-type-hidden
    height: 50px
    width: 50px
    margin: 0 5px
    background: $chosenBackgroundFields
    border-radius: 50%
</style>
