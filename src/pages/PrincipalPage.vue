<template>
  <transition name="fade">
    <q-page>
      <div :class="backgroundClass"></div>
      <div class="overlay"></div>
      <div class="main-content">
        <div class="content">
          <img class="fotoP" src="/fotoPrincipal.jpeg" alt="fotoP" />
          <p class="sacramento">Nayeli</p>
          <p class="junge">MIS 15 AÑOS</p>
          <img class="linead" src="/linea.png" />
        </div>
        <div class="centrado">
          <MusicPlayer class="music-player" />
          <img class="linea" src="/linea2.png" />
          <p class="allura">Aparta la Fecha</p>
          <p class="openSans">9 de Noviembre de 2024</p>
          <p class="openSans" style="margin-top: 10px">Faltan:</p>
          <CuentaRegresiva />
          <img class="lineaAbajo" src="/linea2.png" style="margin-top: 30px" />
        </div>
        <div class="centrado degradado">
          <p class="sacramento2">Nayeli</p>
          <p class="allura parrafoP">
            Sabemos que hoy es un dia muy especial, muy esperado por ti, por
            ello queremos que la realidad de entrar en esta etapa supere a tus
            suenos.<br />
            Felices 15 anos <br />Te queremos mucho. <br />Atte: Tus Padres
          </p>
          <img class="foto1" src="/foto1.jpeg" style="margin-top: 30px" />
        </div>
        <div class="video-background">
          <video autoplay muted loop playsinline>
            <source src="/marcha.mp4" type="video/mp4" />
            Tu navegador no soporta la etiqueta de video.
          </video>
          <div class="contenido">
            <img class="lineav" src="/linea2.png" />
            <p class="allura letra">Ceremonia Religiosa</p>
            <p class="openSans2" style="margin-top: 5px">4:00 p.m.</p>
            <img class="iglesia" src="/iglesia.jpg" />
            <p class="junge2">Iglesia de Nuestro Señor de Ostua</p>
            <q-btn
              style="background: goldenrod"
              glossy
              label="Abrir Ubicacion"
              @click="abrirUbicacion"
            />
            <img class="lineav2" src="/linea2.png" />
          </div>
        </div>
        <div class="video-background">
          <video autoplay muted loop playsinline>
            <source src="/shutup.mp4" type="video/mp4" />
            Tu navegador no soporta la etiqueta de video.
          </video>
          <div class="contenido">
            <img class="lineav" src="/linea2.png" />
            <p class="allura letra">Recepcion</p>
            <p class="openSans2" style="margin-top: 5px">5:00 p.m.</p>
            <img class="iglesia" src="/recepcion.jpg" />
            <p class="junge2">Salon de eventos, Rancho La Potranca</p>
            <q-btn
              style="background-color: goldenrod"
              glossy
              label="Abrir Ubicacion"
            />
            <img class="lineav2" src="/linea2.png" />
          </div>
        </div>
        <div class="content"></div>
      </div>
    </q-page>
  </transition>
</template>

<script setup>
import { ref, onMounted } from "vue";
import CuentaRegresiva from "../components/CuentaRegresiva.vue";
import MusicPlayer from "../components/MusicPlayer.vue";

const backgroundClass = ref("background-inicial");

const seccionCambio1 = ref(null);
const seccionCambio2 = ref(null);
const observerCallback = (entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      if (entry.target === seccionCambio1.value) {
        backgroundClass.value = "background-cambio1";
      } else if (entry.target === seccionCambio2.value) {
        backgroundClass.value = "background-cambio2";
      }
    } else {
      // Restablece la clase de fondo cuando la sección sale de la vista
      if (
        entry.target === seccionCambio1.value ||
        entry.target === seccionCambio2.value
      ) {
        backgroundClass.value = "background-inicial";
      }
    }
  });
};
const observer = new IntersectionObserver(observerCallback, {
  threshold: 0.25,
});

onMounted(() => {
  if (seccionCambio1.value) observer.observe(seccionCambio1.value);
  if (seccionCambio2.value) observer.observe(seccionCambio2.value);
});

const abrirUbicacion = () => {
  const latitud = 14.336533;
  const longitud = -89.44831;

  const wazeUrl = `https://waze.com/ul?ll=${latitud},${longitud}&navigate=yes`;
  const googleMapsUrl = `https://www.google.com/maps/search/?api=1&query=${latitud},${longitud}`;

  window.location.href = wazeUrl;

  setTimeout(() => {
    window.location.href = googleMapsUrl;
  }, 1000);
};
</script>

<style scoped>
.background-inicial {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url("/fotoPierna2.jpeg");
  background-size: cover;
  background-position: center center;
  background-attachment: fixed;
  z-index: -1;
  transition: background-image 0.5s ease-in-out;
}

.background-cambio1 {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url("/fotoLado.jpeg");
  background-size: cover;
  background-position: center center;
  background-attachment: fixed;
  z-index: -1;
  transition: background-image 0.5s ease-in-out;
}

.background-cambio2 {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url("/fotoLado.jpeg");
  background-size: cover;
  background-position: center center;
  background-attachment: fixed;
  z-index: -1;
  transition: background-image 0.5s ease-in-out;
}

/* Resto del estilo sigue igual... */
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
    height: 100vh;
    color: #fff; /* Color del texto para mejor visibilidad sobre el fondo oscuro */
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .background-inicial {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: url("/fotoPierna2.jpeg");
    background-size: cover;
    background-position: center center;
    background-attachment: fixed;
    z-index: -1;
    transition: background-image 0.5s ease-in-out;
  }

  .background-cambio1 {
    background-image: url("/fotoLado.jpeg");
  }

  .background-cambio2 {
    background-image: url("/fotoLado.jpeg");
  }

  .fotoP {
    height: 100vh; /* Ajusta el tamaño según sea necesario */
    width: auto;
    position: absolute;
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
    position: relative; /* Posición absoluta para que el texto se sitúe sobre el cuadro */
    top: 35%; /* Ajusta la posición según sea necesario */
    z-index: 2; /* Asegura que el texto esté sobre el cuadro negro */
    text-shadow: 1px 1px 2px #000;
  }
  .junge {
    font-family: "Junge", cursive;
    font-weight: 400;
    font-style: normal;
    position: relative;
    top: 33%;
    z-index: 2;
    font-size: 25px;
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
    position: relative;
    top: 20%;
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
  .parrafoP {
    font-size: 25px;
    margin-top: 20px;
  }
  .sacramento2 {
    font-family: "Sacramento", cursive;
    font-weight: 500;
    font-style: normal;
    color: rgb(239, 184, 16); /* Color dorado */
    font-size: 100px;
    z-index: 2; /* Asegura que el texto esté sobre el cuadro negro */
    text-shadow: 1px 1px 2px #000;
    left: 70px;
    margin-bottom: 0px;
    margin-top: 600px;
  }
  .degradado {
    background: linear-gradient(
      to bottom,
      rgba(34, 85, 34, 0.1) 0%,
      /* Verde más transparente al inicio */ rgba(34, 85, 34, 0.5) 10%,
      /* Un poco más sólido a medida que baja */ rgba(34, 85, 34, 0.7) 20%,
      /* Aún más sólido */ rgba(34, 85, 34, 1) 50%
        /* Verde completamente sólido al final */
    ); /* Degradado vertical de verde oscuro */
    padding: 20px; /* Espaciado interno */
  }
  .foto1 {
    width: 95%;
    height: auto;
  }
  .video-background {
    position: relative;
    width: 100%;
    height: 90vh; /* O el tamaño que desees */
    overflow: hidden;
    display: flex;
    justify-content: center; /* Centra horizontalmente */
    align-items: center; /* Centra verticalmente */
  }

  .video-background video {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 100%;
    height: 100%;
    object-fit: cover; /* Asegura que el video cubra todo el div */
    transform: translate(-50%, -50%);
    z-index: -1; /* Manda el video al fondo */
  }

  .contenido {
    width: 90%;
    height: 90%;
    background-color: rgba(0, 0, 0, 0.3); /* Fondo semitransparente */
    z-index: 1; /* Asegura que el contenido esté sobre el video */
    color: white; /* Ajusta el color del texto según sea necesario */
    text-align: center;
    padding: 10px;
    display: flex;
    flex-direction: column;

    align-items: center; /* Centra el contenido dentro del div */
  }
  .lineav {
    width: 90%;
    margin-top: 30px;
  }
  .lineav2 {
    width: 90%;
    margin-top: 40px;
  }
  .openSans2 {
    font-family: "Open Sans", sans-serif;
    font-optical-sizing: auto;
    font-weight: 300;
    font-style: normal;
    font-size: 25px;
  }
  .letra {
    font-size: 43px;
    margin-bottom: 0px;
    margin-top: 5px;
  }
  .iglesia {
    width: 95%;
    height: auto;
    margin-top: 15px;
    border: 3px solid goldenrod;
  }
  .junge2 {
    font-size: 20px;
    font-family: "Junge", cursive;
    font-weight: 400;
    font-style: normal;
    margin-top: 50px;
  }
}
</style>
