<template>
  <!-- <div v-if="pokeFound" class="win-overlay"></div> -->

  <!-- <div class="poke-search">
      <h3>Search:</h3>
      <input
        @keyup.enter="pokePick(pokeListFilter[0])"
        ref="inputSearch"
        type="text"
        v-model="filter"
      />
    </div> -->
  <div class="all-pokemon">
    <PokeballPick
      :allowLoadingFade="allowLoadingFade"
      :pokeListFilter="pokeListFilter"
      :pokeSprite="pokeSprite"
      :pokeFind="pokeFind"
      :pokeMissed="pokeMissed"
      :pokeList="pokeList"
      :pokePick="pokePick"
      :pokeFound="pokeFound"
    />
  </div>
</template>

<script>
import PokeballPick from '@/components/PokeballPick.vue';
export default {
  name: 'PokeList',
  props: {
    pokeList: Array,
    pickPokemon: Function,
    pokeSprite: Function,
    chosenPoke: Object,
    pokeFind: Function,
    pokeFound: Boolean,
    pokeMissed: Function,
    allowLoadingFade: Boolean,
  },
  components: { PokeballPick },
  data() {
    return {
      filter: '',
    };
  },
  computed: {
    pokeListFilter() {
      if (this.filter.length > 0) {
        return this.pokeList.filter((p) => p.name.indexOf(this.filter) > -1);
      }
      return this.pokeList;
    },
  },
  methods: {
    pokePick(item) {
      this.pickPokemon(item);
      setTimeout(() => {
        this.filter = '';
        // this.$refs.inputSearch.focus();
      }, 300);
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../variables.scss';

// .poke-list {
//   padding-top: 20px;
//   width: 100%;
//   display: flex;
//   flex-direction: column;
// }
.all-pokemon {
  // max-height: 100%;
  padding: 10px;
  // overflow-y: scroll;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: flex-start;
}
// .win-overlay {
//   width: 100%;
//   height: 100%;
//   position: fixed;
//   z-index: 100;
//   background: $transparent;
// }
// .pokelist-loading {
//   position: absolute;
//   top: 0;
//   width: 100%;
//   height: 100%;
//   overflow: hidden;
//   display: flex;
//   justify-content: center;
//   align-items: center;
//   background: $shadow;
// }
// .list-loading {
//   height: 50px;
//   width: 50px;
// }
// .poke-search {
//   display: flex;
//   height: 30px;
//   margin-left: 60px;

//   h3 {
//     margin: 0 20px;
//     line-height: 30px;
//   }
//   input {
//     height: 30px;
//     width: 300px;
//     outline: none;
//   }
// }
// .list-wrapper {
//   max-height: calc(100% - 30px);
//   width: 100%;
//   position: relative;
//   &::before {
//     content: '';
//     background: $fadeColorBottom;
//     // background: red;
//     position: absolute;
//     width: calc(100% - 30px);
//     height: 20px;
//     top: 0;
//     left: 0;
//     z-index: 10;
//   }
//   &::after {
//     content: '';
//     background: $fadeColorTop;
//     position: absolute;
//     width: calc(100% - 30px);
//     height: 20px;
//     bottom: 0;
//     left: 0;
//     z-index: 10;
//   }
// }
</style>
