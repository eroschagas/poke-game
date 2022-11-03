<template>
  <div class="sect">
    <div ref="teste" class="pick-section">
      <div v-for="(item, index) in clickedPokemon" :key="index">
        <div :id="item.id" :ref="'card-' + item.id" class="pick-card slideLeft">
          <div class="pick-value pick-name">{{ item.name }}</div>
          <div class="pick-sprite fade-in-delay">
            <img
              :src="pokeSprite(item.id - 1, 'other', 'official-artwork')"
              :alt="item.name"
            />
          </div>
          <TypeImage
            class="fade-in-delay"
            :poke="item"
            :clickedPokemon="clickedPokemon"
            :typeSprite="typeSprite"
          />

          <div class="pick-value">
            <p>{{ item.weight / 10 }}KG</p>
            <div>
              <ion-icon
                v-if="item.weight < chosenPoke.weight"
                name="arrow-up-circle-outline"
              ></ion-icon>
              <ion-icon
                v-else-if="item.weight > chosenPoke.weight"
                name="arrow-down-circle-outline"
              ></ion-icon>
              <ion-icon v-else name="checkmark-circle-outline"></ion-icon>
            </div>
          </div>

          <div class="pick-value">
            <p>{{ item.height / 10 }}M</p>
            <div>
              <ion-icon
                v-if="item.height < chosenPoke.height"
                name="arrow-up-circle-outline"
              ></ion-icon>
              <ion-icon
                v-else-if="item.height > chosenPoke.height"
                name="arrow-down-circle-outline"
              ></ion-icon>
              <ion-icon v-else name="checkmark-circle-outline"></ion-icon>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="smooth"></div>
  </div>
</template>

<script>
import TypeImage from '@/components/TypeImage.vue';

export default {
  name: 'PokePick',
  props: ['clickedPokemon', 'pokeSprite', 'chosenPoke', 'typeSprite'],
  computed: {
    listMin() {
      if (this.clickedPokemon.length >= 4) {
        return this.clickedPokemon.length - 4;
      }
      return 0;
    },
    listMax() {
      return this.clickedPokemon.length;
    },
  },
  watch: {
    clickedPokemon: {
      deep: true,
      handler() {
        this.$nextTick(() => {
          var lastCardHeight =
            this.$refs[
              'card-' + this.clickedPokemon[this.clickedPokemon.length - 1].id
            ][0].offsetHeight;
          this.$refs.teste.scrollTo(
            0,
            this.clickedPokemon.length * lastCardHeight
          );
        });
      },
    },
  },
  components: { TypeImage },
};
</script>

<style lang="sass">
@import '../variables.sass'

.pick-section
  height: 256px
  width: fit-content
  overflow-y: hidden
  overflow-x: hidden
  scroll-behavior: smooth

.sect
  position: relative
  height: 256px
  width: fit-content
  margin: 20px

.smooth
  position: absolute
  top: 0
  right: 0
  display: block
  height: 100%
  width: 40px
  z-index: 100
  background-image: linear-gradient(to right, $transparent, white)

.pick-card
  display: flex
  justify-content: flex-start
  align-items: center

.pick-sprite
  margin: 10px
  img
    height: 40px
    width: 40px
.pick-value
  margin: 10px 20px 10px 10px
  width: 90px
  height: 64px
  background: $transparent
  display: flex
  justify-content: flex-end
  align-items: center
  border-radius: 32px
  div
    margin: 10px
    display: flex
    justify-content: flex-end
    align-items: center
    font-size: 24px
  p
    text-align: right
    font-family: $pixel
    min-width: 80px

.pick-name
  width: 130px
  font-family: $pixel
</style>
