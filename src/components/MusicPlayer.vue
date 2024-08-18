<template>
  <div class="music-player">
    <p class="junge">Escucha mi canción</p>
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
    <button @click="previousTrack" class="control-button">
      <span class="material-symbols-outlined"> skip_previous </span>
    </button>
    <button @click="togglePlayPause" class="control-button">
      <span class="material-icons">
        {{ isPlaying ? "pause" : "play_arrow" }}
      </span>
    </button>
    <button @click="nextTrack" class="control-button">
      <span class="material-symbols-outlined"> skip_next </span>
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const isPlaying = ref(true);
const currentTime = ref(0);
const duration = ref(0);
const audio = new Audio("/goldenhour.mp3"); // Ruta de tu canción

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
  flex-direction: column; /* Alinea los elementos en columna */
  align-items: center; /* Centra los elementos horizontalmente */
  gap: 10px;
  color: #fff;
  margin-top: 50px;
  justify-content: center;
  width: 70%; /* Asegura que el contenedor ocupe todo el ancho disponible */
}

.control-button {
  background-color: transparent;
  border: none;
  color: #fff;
  font-size: 1.5rem;
  cursor: pointer;
}
.progress-bar {
  width: 100%;
  background: #444; /* Color de fondo de la barra */
  border-radius: 5px;
  height: 8px; /* Altura de la barra */
  cursor: pointer;
  appearance: none;
}

.progress-bar::-webkit-slider-runnable-track {
  background: linear-gradient(
    145deg,
    #d4af37,
    #c7971e
  ); /* Gradiente para la parte recorrida de la barra */
  height: 8px; /* Altura de la barra recorrida */
  border-radius: 5px;
}

.progress-bar::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 12px;
  height: 12px;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  position: relative;
  z-index: 3;
  top: -2px;
}

.progress-bar::-moz-range-thumb {
  width: 15px;
  height: 15px;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  position: relative;
  z-index: 3;
}

.junge {
  font-family: "Junge", cursive;
  font-weight: 400;
  font-style: normal;
  font-size: 24px; /* Aumenta el tamaño del texto */
  color: #fff;
  text-align: center; /* Centra el texto horizontalmente */
  margin-bottom: 20px; /* Añade un margen inferior para separar del resto del contenido */
}
</style>
