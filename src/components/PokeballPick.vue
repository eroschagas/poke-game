<template>
  <div
    :class="allowLoadingFade ? 'poke-cards' : ''"
    v-for="(item, index) in pokeListFilter"
    :key="index"
  >
    <div
      @click="pokePick(item)"
      :class="classCard(item)"
      v-if="pokeList.length"
    >
      <div>
        <div class="pokeball-full pokeball-top"></div>
        <div class="pokeball-full pokeball-bot"></div>
        <img
          :class="(allowLoadingFade ? 'fade-in-delay' : '', 'hide-alt')"
          :src="pokeSprite(item.id)"
          :alt="item.name"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokeballPick',
  props: [
    'allowLoadingFade',
    'pokeListFilter',
    'pokeSprite',
    'pokeMissed',
    'pokeFind',
    'pokePick',
    'pokeList',
    'pokeFound',
  ],
  methods: {
    classCard(item) {
      let card = ['poke-card', 'fade-in'];
      if (this.pokeMissed(item)) {
        card.push('poke-card-disabled-animated');
      } else {
        card.push('poke-card-hover');
      }
      if (this.pokeFind(item)) {
        card.push('poke-card-found');
      }
      return card;
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../variables.scss';

.poke-cards {
  opacity: 0;
  transition: all 1s ease-out;
  // animation: fadeIn 0.5s 2s forwards ease-out
}
.poke-card {
  width: 90px;
  height: 90px;
  border-radius: 50%;
  margin: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.1s ease-in-out;
  cursor: pointer;
  position: relative;

  div {
    width: 90px;
    height: 90px;
    display: flex;
    justify-content: center;
    align-items: center;

    img {
      image-rendering: pixelated;
      // position: relative;
      border-radius: 50%;
      background-image: radial-gradient($pokeBackground 30%, transparent 70%);
      width: 70px;
      height: 70px;
      z-index: 100;
    }
  }
}
.poke-card-hover:hover {
  // background: $pokelistBackgroundHover
  scale: (1.2);

  div {
    opacity: 1;
    animation-name: shake, grow-shrink;
    animation-duration: 0.4s, 0.5s;
    animation-timing-function: ease-in-out, ease-in-out;
    animation-iteration-count: 1, 1;
  }
}
.poke-card-found {
  position: relative;
  z-index: 10;
  background: $pokelistBackgroundWin;
  transform: scale(1.4);
  transition: all 0.5s ease-in-out;
  // &:hover
  //   background: $pokelistBackgroundWin;
  //   transform: scale(1.3);
}
.pokeball-full {
  width: 90px;
  height: 90px;
  position: absolute;
  top: 0;
  left: 0;
}
.pokeball-top {
  background-image: url('../assets/Poke_Ball_icon_top.svg');
}
.pokeball-bot {
  background-image: url('../assets/Poke_Ball_icon_bot.svg');
}
.poke-card-disabled-animated {
  cursor: not-allowed;

  img {
    transition: all 0.5s ease-in-out;
    filter: brightness(0);
  }

  .pokeball-top {
    transition: all 0.3s ease-in-out;
    translate: 0 -5px;
  }
  .pokeball-bot {
    transition: all 0.3s ease-in-out;
    translate: 0 5px;
  }
}
.poke-card-disabled {
  cursor: not-allowed;

  img {
    filter: brightness(0);
  }

  .pokeball-top {
    translate: 0 -5px;
  }
  .pokeball-bot {
    translate: 0 5px;
  }
}
</style>
