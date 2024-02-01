<script setup lang="ts">
import { inject } from "@vercel/analytics";
import { ref } from "vue";
import { injectSpeedInsights } from "@vercel/speed-insights";

injectSpeedInsights();
inject();

let isPlaying = ref(false);
const oscillator = ref<OscillatorNode | null>(null);

const initialize = () => {
  let audioContext = new AudioContext();
  const osc = audioContext.createOscillator();
  osc.connect(audioContext.destination);
  oscillator.value = osc;
  osc.type = "sine";
  osc.frequency.value = 11000;
};

const play = () => {
  if (!isPlaying.value) {
    initialize();
    oscillator.value?.start();
    isPlaying.value = true;
  }
};

const stop = () => {
  if (isPlaying.value) {
    oscillator.value?.stop();
    isPlaying.value = false;
  }
};
</script>

<template>
  <div class="app">
    <header>
      <h1>Thanos Snap</h1>
    </header>
    <section class="app-start">
      <button @click="play" :class="{ disabled: isPlaying }">Snap!!!</button>
      <button @click="stop" :class="{ disabled: !isPlaying }">
        Call Avengers!
      </button>
    </section>
    <footer>
      <p class="footer-text">
        This app is just for educational purpose. I do not own any rights of the
        images used in this app. I am not affiliated with Marvel or Disney, and
        this app is not official. If used to cause harm to others, I am not
        responsible. Use this app responsibly and at your own risk.
      </p>
    </footer>
  </div>
</template>

<style scoped>
button {
  padding: 1rem 2rem;
  border: none;
  border-radius: 0.5rem;
  font-size: 1.5rem;
  font-weight: bold;
  cursor: pointer;
}

.app {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
  height: 100dvh;
  width: 100dvw;
  background-image: url(./assets/back.jpg);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  align-items: center;
  align-content: center;
  flex-wrap: wrap;
  flex-direction: column;
}

.footer-text {
  font-size: 0.8rem;
  text-align: justify;
}

.disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.app-start {
  display: flex;
  flex-direction: row;
  gap: 1rem;
}

footer {
  color: white;
  background-color: rgba(0, 0, 0, 0.187);
  backdrop-filter: blur(20px);
  width: 100dvw;
  padding: 0 1rem;
  box-sizing: border-box;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 10vh;
  width: 100vw;
  color: white;
  background-color: rgba(0, 0, 0, 0.187);
  backdrop-filter: blur(20px);
}
</style>
