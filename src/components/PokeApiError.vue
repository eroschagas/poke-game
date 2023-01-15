<template>
  <transition name="api-error">
    <div v-if="!this.pokeApi" class="error-body">
      <div class="error-img">
        <img src="../assets/weedle-gif.gif" alt="weedle-gif" />
      </div>
      <div class="error-graphql">
        <h1>PokeApi GraphQL server is offline!</h1>
        <p>Please come back later...</p>
      </div>
      <p>
        This website uses a free api at
        <a href="https://pokeapi.co/docs/graphql" target="_blank"
          >https://pokeapi.co/docs/graphql</a
        >
      </p>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'PokeApiError',
  props: ['pokeApi'],
};
</script>

<style lang="scss">
@import '../variables.scss';
@keyframes moveWeedle {
  0% {
    translate: calc(100% - 100px);
  }
  49% {
    scale: 1 1;
    translate: 0;
  }
  50% {
    scale: -1 1;
    translate: calc(-100% + 100px);
  }
  99% {
    scale: -1 1;
    translate: 0;
  }
  100% {
    scale: 1 1;
    translate: calc(100% - 100px);
  }
}

.error-body {
  height: 100vh;
  width: 100vw;
  background: white;
  position: fixed;
  z-index: 100000000;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  p {
    text-align: center;
    margin-top: 8px;

    a {
      white-space: nowrap;
    }
  }
}
.error-img {
  max-width: 800px;
  width: 100%;
  translate: calc(100% - 100px);
  animation: moveWeedle 10s infinite linear;

  img {
    width: 100px;
    image-rendering: pixelated;
    margin-bottom: -10px;
  }
}
.error-graphql {
  background: $pokelistBackgroundHover;
  height: fit-content;
  padding: 10px;
  text-align: center;
  max-width: 800px;
  width: calc(100% - 40px);
  margin: 0 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
  border-radius: 30px;
  box-shadow: 2px 2px 4px 2px $shadow;
}
.api-error-enter-active,
.api-error-leave-active {
  transition: opacity 1s ease;
}
.api-error-enter-from,
.api-error-leave-to {
  opacity: 0;
}
</style>
