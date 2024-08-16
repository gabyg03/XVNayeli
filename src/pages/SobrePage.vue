<template>
  <transition name="fade">
    <div v-if="visible" class="envelope-page">
      <div class="envelope"></div>
      <img class="corona" src="src/assets/corona.png" alt="corona" />
      <div class="centerT">
        <p class="textoN">Nayeli</p>
        <p class="textoSN">Te invita a celebrar sus XV</p>
      </div>
      <button class="seal-button" @click="Abrir">
        <img src="src/assets/sello.png" alt="Sello" />
      </button>
      <img class="flores" src="src/assets/flores.png" alt="flores" />
      <img class="flores2" src="src/assets/flores.png" alt="flores2" />
      <img class="flores3" src="src/assets/flores.png" alt="flores3" />
      <img class="flores4" src="src/assets/flores.png" alt="flores4" />
      <img class="rosas-l" src="src/assets/rosasl.png" alt="rosasl" />
      <img class="brillos" src="src/assets/brillosd.png" alt="brillos" />
      <img class="brillos2" src="src/assets/brillosd.png" alt="brillos" />
      <img class="brillos3" src="src/assets/brillosd.png" alt="brillos" />
    </div>
  </transition>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
// Importa el archivo de sonido usando la sintaxis de ES6
import soundUrl from "src/assets/brillos.mp3";

const visible = ref(true);
const router = useRouter();
const audio = new Audio(soundUrl);

const Abrir = () => {
  visible.value = false;
  audio.play();
  setTimeout(() => {
    router.push("/nayelixv");
  }, 500); // Duración de la transición más tiempo del sonido
};
</script>

<style scoped>
@keyframes pulse {
  0% {
    transform: translateX(-50%) scale(1);
  }
  50% {
    transform: translateX(-50%) scale(1.2);
  }
  100% {
    transform: translateX(-50%) scale(1);
  }
}

@keyframes enlarge-reduce {
  0% {
    transform: translateX(-50%) scale(1);
  }
  50% {
    transform: translateX(-50%) scale(1.2);
  }
  100% {
    transform: translateX(-50%) scale(1);
  }
}

.envelope {
  width: 100vw;
  height: 100vh;
  position: relative;
  cursor: pointer;
  perspective: 1024px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: url("src/assets/sobre.png"); /* Asegúrate de colocar la imagen en la carpeta assets */
  background-size: cover;
  background-position: center;
}
.centerT {
  position: absolute;
  top: 48%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1;
}
.textoN {
  font-family: "Sevillana", cursive;
  font-weight: 400;
  font-size: 80px;
  font-style: normal;
  color: #333; /* Ajusta el color según sea necesario */
  text-align: center;
  margin-bottom: 0px;
}
.textoSN {
  font-family: "Glass Antiqua", cursive;
  font-weight: 400;
  font-style: normal;
  font-size: 25px;
  margin-top: 5px;
}
.corona {
  position: absolute;
  top: 32%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1;
  width: 350px;
  height: auto;
}
.seal-button {
  position: absolute;
  top: 62%;
  left: 50%;
  transform: translateX(-50%);
  background: none;
  border: none;
  cursor: pointer;
  z-index: 2;
  animation: pulse 1.5s infinite;
}

.seal-button img {
  width: 150px; /* Ajusta el tamaño según sea necesario */
  height: auto;
}

.flores {
  position: absolute;
  bottom: -50px;
  left: 300px;
  transform: translateX(-50%) rotate(45deg);
  width: 250px; /* Ajusta el tamaño según sea necesario */
  height: auto;
  z-index: 1;
}
.flores2 {
  position: absolute;
  bottom: 180px;
  left: 50px;
  transform: translateX(-50%) rotate(90deg);
  width: 250px; /* Ajusta el tamaño según sea necesario */
  height: auto;
  z-index: 2;
}
.flores3 {
  position: absolute;
  bottom: 325px;
  left: 17px;
  transform: translateX(-50%) rotate(-35deg);
  width: 200px; /* Ajusta el tamaño según sea necesario */
  height: auto;
  z-index: 1;
}
.flores4 {
  position: absolute;
  bottom: -65px;
  left: 450px;
  transform: translateX(-50%) rotate(60deg);
  width: 200px; /* Ajusta el tamaño según sea necesario */
  height: auto;
  z-index: 1;
}
.rosas-l {
  position: absolute;
  top: 86%;
  left: -50px;
  transform: translateY(-50%);
  width: 350px; /* Ajusta el tamaño según sea necesario */
  height: auto;
  z-index: 2;
}

.brillos {
  position: absolute;
  top: 10%;
  right: -50px;
  transform: translateY(-50%) scaleX(-1);
  width: 500px; /* Ajusta el tamaño según sea necesario */
  height: auto;
  z-index: 1;
}
.brillos2 {
  position: absolute;
  top: 30%;
  right: -50px;
  transform: translateY(-50%) rotate(90deg) scaleX(-1);
  width: 350px; /* Ajusta el tamaño según sea necesario */
  height: auto;
  z-index: 1;
}
.brillos3 {
  position: absolute;
  top: 5%;
  right: 170px;
  transform: translateY(-50%) rotate(90deg) scaleX(-1);
  width: 350px; /* Ajusta el tamaño según sea necesario */
  height: auto;
  z-index: 1;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

/* Media queries para dispositivos móviles */
@media (max-width: 600px) {
  .envelope {
    background-size: 900px; /* Ajustar el tamaño de la imagen para que se vea mejor en pantallas pequeñas */
    background-repeat: no-repeat; /* Evitar la repetición de la imagen */
  }
  .centerT {
    position: absolute;
    top: 49%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 1;
  }
  .textoN {
    font-family: "Sevillana", cursive;
    font-weight: 400;
    font-size: 50px;
    font-style: normal;
    color: #333; /* Ajusta el color según sea necesario */
    text-align: center;
    margin-bottom: 0px;
  }
  .textoSN {
    font-family: "Glass Antiqua", cursive;
    font-weight: 400;
    font-style: normal;
    font-size: 20px;
    margin-top: 5px;
  }
  .corona {
    position: absolute;
    top: 35%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 1;
    width: 230px;
    height: auto;
  }
  .seal-button {
    position: absolute;
    top: 57%;
    left: 50%;
    transform: translateX(-50%);
    background: none;
    border: none;
    cursor: pointer;
    z-index: 2;
  }

  .seal-button img {
    width: 90px; /* Ajusta el tamaño según sea necesario */
    height: auto;
  }

  .rosas-ab {
    position: absolute;
    top: -30px;
    left: 78%;
    transform: translateX(-50%) rotate(180deg);
    width: 300px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    z-index: 2;
  }

  .rosas-l {
    position: absolute;
    top: 90%;
    left: -40px;
    transform: translateY(-50%);
    width: 250px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    z-index: 2;
  }
  /*brillo izquierda*/
  .brillos {
    position: absolute;
    top: 0;
    left: -149px;
    transform: translateY(-50%) scaleX(-1);
    width: 350px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    z-index: 1;
  }
  /*brillo centro*/
  .brillos2 {
    position: absolute;
    top: 0;
    left: 115px;
    transform: translateY(-50%);
    width: 250px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    z-index: 1;
  }
  /*brillo derecho*/
  .brillos3 {
    position: absolute;
    top: 0;
    left: 210px;
    transform: translateY(-50%);
    width: 350px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    z-index: 1;
  }
  .flores {
    position: absolute;
    bottom: -75px;
    left: 49%;
    transform: translateX(-50%) rotate(35deg);
    width: 200px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    z-index: 1;
  }
  .flores2 {
    position: absolute;
    bottom: 130px;
    left: 10%;
    transform: translateX(-50%) rotate(85deg);
    width: 200px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    z-index: 1;
  }
  .flores3 {
    position: absolute;
    bottom: 250px;
    left: 3%;
    transform: translateX(-50%) rotate(85deg);
    width: 150px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    z-index: 1;
  }
  .flores4 {
    position: absolute;
    bottom: -82px;
    left: 80%;
    transform: translateX(-50%) rotate(35deg);
    width: 150px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    z-index: 1;
  }
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.5s;
  }
  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }
}
</style>
