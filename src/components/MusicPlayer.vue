<template>
  <div class="music-player">
    <div class="progress-container">
      <input
        type="range"
        min="0"
        :max="duration"
        v-model="currentTime"
        @input="seekTrack"
        class="progress-bar"
      />
    </div>
    <button @click="togglePlayPause" class="control-button">
      <span class="material-icons">
        {{ isPlaying ? "pause" : "play_arrow" }}
      </span>
    </button>
    <button @click="previousTrack" class="control-button">
      <span class="material-symbols-outlined"> skip_next </span>
    </button>

    <button @click="nextTrack" class="control-button">
      <span class="material-symbols-outlined"> skip_previous </span>
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const isPlaying = ref(true);
const currentTime = ref(0);
const duration = ref(0);
const audio = new Audio("src/assets/goldenhour.mp3"); // Ruta de tu canción

const togglePlayPause = () => {
  if (isPlaying.value) {
    audio.pause();
  } else {
    audio.play();
  }
  isPlaying.value = !isPlaying.value;
};

const updateProgress = () => {
  currentTime.value = audio.currentTime;
};

const seekTrack = () => {
  audio.currentTime = currentTime.value;
};

const previousTrack = () => {
  // Aquí puedes agregar lógica para cambiar a la pista anterior
  audio.currentTime = 0;
  updateProgress();
};

const nextTrack = () => {
  // Aquí puedes agregar lógica para cambiar a la siguiente pista
  audio.currentTime = 0;
  updateProgress();
};

onMounted(() => {
  audio.play();
  audio.addEventListener("timeupdate", updateProgress);
  duration.value = audio.duration;
});

onUnmounted(() => {
  audio.removeEventListener("timeupdate", updateProgress);
  audio.pause();
});
</script>

<style scoped>
.music-player {
  display: inline;
  align-items: center;
  gap: 10px;
  color: #fff;
  margin-top: 50px;
}

.control-button {
  background-color: transparent;
  border: none;
  color: #fff;
  font-size: 1.5rem;
  cursor: pointer;
}

.progress-container {
  flex: 1;
}

.progress-bar {
  width: 100%;
  background: linear-gradient(145deg, #d4af37, #c7971e);
  border-radius: 5px;
  height: 5px;
  cursor: pointer;
}

.progress-bar::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 15px;
  height: 15px;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  cursor: pointer;
}

.progress-bar::-moz-range-thumb {
  width: 15px;
  height: 15px;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  cursor: pointer;
}
</style>
