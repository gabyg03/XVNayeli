<template>
  <transition name="fade">
    <q-page>
      <div class="background-image"></div>
      <div class="overlay"></div>
      <div class="main-content">
        <div class="content">
          <img class="fotoP" src="src/assets/fotoPrincipal.jpeg" alt="fotoP" />
          <p class="sacramento">Nayeli</p>
          <p class="junge">MIS 15 AÑOS</p>
          <img class="linead" src="src/assets/linea.png" />
        </div>
        <div class="centrado">
          <MusicPlayer class="music-player" />
          <img class="linea" src="src/assets/linea2.png" />
          <p class="allura">Aparta la Fecha</p>
          <p class="openSans">9 de Noviembre de 2024</p>
          <CuentaRegresiva />
          <p class="openSans" style="margin-top: 30px">Faltan:</p>
          <img class="lineaAbajo" src="src/assets/linea2.png" />
        </div>
      </div>
    </q-page>
  </transition>
</template>

<script setup>
import CuentaRegresiva from "../components/CuentaRegresiva.vue";
import MusicPlayer from "../components/MusicPlayer.vue";
</script>

<style scoped>
.background-image {
  position: fixed; /* Hacer que el fondo sea fijo */
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url("src/assets/fotoPierna2.jpeg");
  background-size: cover;
  background-position: center center;
  background-attachment: fixed; /* Asegura que la imagen de fondo no se mueva */
  z-index: -1; /* Asegúrate de que esté detrás de todo el contenido */
}

.main-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  box-sizing: border-box;
}

.fotoP {
  height: 100vh;
  width: auto;
  position: relative;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); /* Color negro con 50% de opacidad */
  z-index: 0; /* Asegura que el overlay esté sobre la foto, pero debajo del texto */
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to /* .fade-leave-active en <2.1.8 */ {
  opacity: 0;
}

@media (max-width: 600px) {
  .main-content {
    position: relative; /* Importante para que el contenido se posicione correctamente sobre el fondo */
    flex-direction: column;
    justify-content: center;
    width: 100%;
    min-height: 100vh; /* Asegura que el contenido ocupe al menos la altura de la ventana */
    color: #fff; /* Color del texto para mejor visibilidad sobre el fondo oscuro */
    display: flexbox;
    align-items: center;
    justify-content: center;
  }
  .content {
    position: relative; /* Importante para que el contenido se posicione correctamente sobre el fondo */
    flex-direction: column;
    justify-content: center;
    width: 100%;
    color: #fff; /* Color del texto para mejor visibilidad sobre el fondo oscuro */
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .background-image {
    background-image: url("src/assets/fotoPierna3.jpeg");
    background-position: center center;
  }

  .fotoP {
    height: 915px; /* Ajusta el tamaño según sea necesario */
    width: auto;
  }
  .overlay {
    position: fixed; /* Hacer que el fondo sea fijo */
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1; /* Asegúrate de que esté detrás de todo el contenido */
  }

  .sacramento {
    font-family: "Sacramento", cursive;
    font-weight: 500;
    font-style: normal;
    color: rgb(239, 184, 16); /* Color dorado */
    font-size: 100px;
    position: absolute; /* Posición absoluta para que el texto se sitúe sobre el cuadro */
    bottom: 250px; /* Ajusta la posición según sea necesario */
    z-index: 2; /* Asegura que el texto esté sobre el cuadro negro */
    text-shadow: 1px 1px 2px #000;
    left: 70px;
  }
  .junge {
    font-family: "Junge", cursive;
    font-weight: 400;
    font-style: normal;
    position: absolute;
    bottom: 200px;
    z-index: 2;
    font-size: 25px;
    left: 145px;
  }
  .allura {
    font-family: "Allura", cursive;
    font-weight: 400;
    font-style: normal;
    font-size: 50px;
    margin-top: 20px;
  }

  .lineaAbajo {
    width: 300px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    top: 770px; /* Ajusta la posición según sea necesario */
    z-index: 2;
  }
  .linead {
    width: 450px;
    height: auto;
    position: absolute;
    bottom: 30px;
  }
  .contenedor-principal > div {
    width: 100%; /* Abarca todo el ancho del contenedor */
    max-width: 1200px; /* Opcional: límite de ancho para evitar que sea muy ancho en pantallas grandes */
    margin-bottom: 16px; /* Espaciado entre los divs */
  }
  .centrado {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    box-sizing: border-box;
    text-align: center; /* Centra el texto en líneas múltiples */
  }

  .linea {
    width: 300px; /* Ajusta el tamaño según sea necesario */
    height: auto;
    margin-top: 200px; /* Ajusta la posición según sea necesario */
    z-index: 2;
  }
  .openSans {
    font-family: "Open Sans", sans-serif;
    font-optical-sizing: auto;
    font-weight: 300;
    font-style: normal;
    font-size: 30px;
  }
}
</style>
