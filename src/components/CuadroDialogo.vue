<template>
  <div>
    <q-btn
      style="background: goldenrod"
      glossy
      label="Confirmar Asistencia"
      @click="abrirDialogo"
    />
  </div>
</template>

<script setup>
import Swal from "sweetalert2";
import { ref } from "vue";
import emailjs from "emailjs-com";
import invitaciones from "/public/invitaciones.json"; // Ruta del archivo JSON

// Función para enviar el correo con EmailJS
const enviarCorreo = (codigo, familia, asistentes) => {
  const templateParams = {
    codigo: codigo,
    nombre: familia.invitado,
    reservado: familia.reservado,
    asistentes: asistentes,
  };

  // Configura tu servicio de EmailJS
  emailjs
    .send(
      "service_8f6n1md", // Cambia esto por tu service ID de EmailJS
      "template_0jerpzf", // Cambia esto por tu template ID de EmailJS
      templateParams,
      "CTO5MmmM1SoQPYJ2X" // Cambia esto por tu user ID de EmailJS
    )
    .then(
      (response) => {
        console.log(
          "Correo enviado con éxito!",
          response.status,
          response.text
        );
        Swal.fire(
          "Éxito",
          "La confirmación ha sido enviada por correo.",
          "success"
        );
      },
      (error) => {
        console.error("Error al enviar el correo:", error);
        Swal.fire("Error", "Hubo un problema al enviar el correo.", "error");
      }
    );
};

const abrirDialogo = async () => {
  // Preguntar por el código de confirmación
  const { value: codigo } = await Swal.fire({
    title: "Ingresar código de confirmación",
    input: "text",
    inputLabel: "Código de familia",
    showCancelButton: true,
    confirmButtonText: "Siguiente",
    inputValidator: (value) => {
      if (!value) {
        return "Por favor, ingrese un código";
      }
    },
  });

  if (codigo) {
    // Buscar el código en el archivo JSON
    const familia = invitaciones.find((inv) => inv.codigo === codigo);

    if (familia) {
      // Preguntar cuántos asistirán sin exceder el número reservado
      const { value: asistentes } = await Swal.fire({
        title: `Familia: ${familia.invitado}`,
        html: `Tienen reservados ${familia.reservado} espacios`,
        input: "number",
        inputLabel: "Número de asistentes",
        inputAttributes: {
          min: 1,
          max: familia.reservado,
        },
        confirmButtonText: "Confirmar",
        showCancelButton: true,
        inputValidator: (value) => {
          if (value > familia.reservado) {
            return `No puede exceder de ${familia.reservado} asistentes`;
          }
          if (value < 0) {
            return `Ingrese una cantidad valida de invitados`;
          }
          if (!value) {
            return `Debe de ingresar al menos 1 invitado`;
          }
          if (!Number.isInteger(value)) {
            return `Debe de ingresar un valor sin "." o ","`;
          }
        },
      });

      if (asistentes) {
        // Confirmación de asistencia
        Swal.fire(`Asistencia confirmada para ${asistentes} personas`);

        // Enviar el correo de confirmación
        enviarCorreo(codigo, familia, asistentes);
      }
    } else {
      Swal.fire("Error", "Código no encontrado", "error");
    }
  }
};
</script>

<style>
.swal2-popup {
  border: 2px solid gold; /* Orilla dorada */
  background-color: #f40909; /* Fondo rojo ocre */
}
</style>
