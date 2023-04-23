<template>
  <div class="loading">
    <div class="loading-container">
      <img src="../assets/meowth-gif2.gif" alt="meouth-loading" />
      <div class="loading-wrapper">
        <LoadingIcon class="loading-change" size="50" />
      </div>
      <h3>LOADING</h3>
    </div>
  </div>
</template>

<script>
import LoadingIcon from '../components/LoadingIcon.vue';

export default {
  name: 'LoadingPage',
  components: {
    LoadingIcon,
  },
};
</script>

<style lang="scss" scoped>
@import '../variables.scss';
.loading {
  position: fixed;
  z-index: 100000;
  width: 100vw;
  height: 100vh;
  background: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  img {
    height: 150px;
  }
  h3 {
    padding-top: 20px;
    font-family: $pixel;
  }
}
.loading-wrapper {
  background-image: url(../assets/Poke_Ball_icon.svg);
  padding-top: 3px;
  padding-left: 1px;
  height: 75px;
  width: 75px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 100px;
  left: 27%;
}
.loading-change {
  rotate: -1deg;
}
.loading-container {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
