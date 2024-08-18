<template>
  <div id="countdown" class="centrado">
    <div>
      <p id="days">{{ days }}</p>
      <p>DÍAS</p>
    </div>
    <div>
      <p id="hours">{{ hours }}</p>
      <p>HRS.</p>
    </div>
    <div>
      <p id="minutes">{{ minutes }}</p>
      <p>MIN.</p>
    </div>
    <div>
      <p id="seconds">{{ seconds }}</p>
      <p>SEG.</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";

const days = ref(0);
const hours = ref(0);
const minutes = ref(0);
const seconds = ref(0);
let countdownInterval = null;

const startCountdown = () => {
  const targetDate = new Date("November 9, 2024 16:00:00").getTime();

  countdownInterval = setInterval(() => {
    const now = new Date().getTime();
    const timeLeft = targetDate - now;

    days.value = Math.floor(timeLeft / (1000 * 60 * 60 * 24));
    hours.value = Math.floor(
      (timeLeft % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
    );
    minutes.value = Math.floor((timeLeft % (1000 * 60 * 60)) / (1000 * 60));
    seconds.value = Math.floor((timeLeft % (1000 * 60)) / 1000);

    if (timeLeft < 0) {
      clearInterval(countdownInterval);
      days.value = hours.value = minutes.value = seconds.value = 0;
    }
  }, 1000);
};

onMounted(() => {
  startCountdown();
});

onBeforeUnmount(() => {
  clearInterval(countdownInterval);
});
</script>

<style scoped>
#countdown {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  gap: 20px;
  font-family: "Arial", sans-serif;
  color: #fff;
}

#countdown div {
  background-color: rgba(0, 0, 0, 0.7);
  padding: 20px;
  border-radius: 10px;
}

#countdown p {
  margin: 0;
  font-size: 2rem;
}
@media (max-width: 600px) {
  #countdown {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    gap: 15px;
    font-family: "Arial", sans-serif;
    color: #fff;
    margin-top: 30px;
    flex-direction: row; /* Asegura que los elementos hijos se dispongan en una fila horizontal */
  }

  #countdown div {
    background: linear-gradient(
      145deg,
      #f4c542,
      #d4a132
    ); /* Degradado dorado */
    padding: 15px;
    border-radius: 15px; /* Bordes más redondeados */
    box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.3); /* Sombra para darle profundidad */
  }

  #countdown p {
    margin: 0;
    font-size: 0.9rem; /* Tamaño de fuente más grande */
    font-weight: bold;
  }
}
</style>
