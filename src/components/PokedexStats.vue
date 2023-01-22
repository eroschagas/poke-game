<template>
  <div v-if="type == 'type'" class="pokedex-stats">
    <div class="pick-stats-container">
      <div class="pick-stats">
        <TypeSprite :pokemon="pokemon" :typeSprite="typeSprite" />
      </div>
    </div>
  </div>
  <div v-if="type == 'stat' && name" :class="['stat', name]">
    <p :style="fontResize">{{ name.toUpperCase() }}</p>
  </div>
  <div v-if="type == 'stat' && name" :class="['analyzer', name]">
    <ion-icon
      v-if="chosen == stat"
      :style="fontResizeIcon"
      name="checkmark-circle-outline"
    ></ion-icon>
    <ion-icon
      v-else-if="chosen > stat"
      :style="fontResizeIcon"
      name="arrow-up-circle-outline"
    ></ion-icon>
    <ion-icon
      v-else-if="chosen < stat"
      :style="fontResizeIcon"
      name="arrow-down-circle-outline"
    ></ion-icon>
  </div>
  <div v-if="type == 'stat' && name && stat" :class="['stat', 'value', name]">
    <p v-if="name == 'weight'" :style="fontResize">
      {{ `${stat}kg` }}
    </p>
    <p v-else-if="name == 'height'" :style="fontResize">
      {{ `${stat}m` }}
    </p>
    <p v-else :style="fontResize">
      {{ stat }}
    </p>
  </div>
</template>

<script>
import TypeSprite from '@/components/TypeSprite.vue';

export default {
  name: 'PokedexStats',
  components: { TypeSprite },
  props: {
    type: String,
    name: String,
    stat: [String, Number],
    pokemon: Object,
    typeSprite: Function,
    fontResize: Object,
    fontResizeIcon: Object,
    chosen: Number,
  },
};
</script>

<style lang="scss" scoped>
@import '../variables.scss';
.pokedex-stats {
  height: 8.5%;
  width: 34.9%;
  animation: screen 0.5s infinite linear, glow 0.5s infinite linear;
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
  animation: screen 0.5s infinite linear, glow 0.5s infinite linear;
  color: white;
  position: absolute;
  top: 60.99%;
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
.analyzer {
  width: 10.4%;
  height: 5%;
  color: black;
  position: absolute;
  top: 60.99%;
  left: 67.53%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}
.weight {
  top: 72.37%;
}
.height {
  top: 83.76%;
}
.value {
  left: 76.8%;
  width: 15.8%;
}
</style>
