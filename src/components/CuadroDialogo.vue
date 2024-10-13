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
const enviarCorreo = (codigo, familia, asistentesAdultos, asistentesNinos) => {
  const templateParams = {
    codigo: codigo,
    nombre: familia.invitado,
    reservado: `${familia.adultos} adultos y ${familia.ninos} niños`,
    asistentes: `${asistentesAdultos} adultos y ${asistentesNinos} niños`,
  };

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
    const familia = invitaciones.find((inv) => inv.codigo === codigo);

    if (familia) {
      let mensajeReservado = `Tienen reservados ${
        familia.adultos + familia.ninos
      } espacios: ${familia.adultos} adultos`;
      if (familia.ninos > 0) {
        mensajeReservado += ` y ${familia.ninos} niños.`;
      } else {
        mensajeReservado += ".";
      }

      // Crear los campos de entrada y estructurar el formulario
      const htmlForm = `
        <div style="display: flex; flex-direction: column; align-items: center; gap: 10px;">
          <div>
            <label>Adultos (máx ${familia.adultos}):</label>
            <input type="number" id="swal-input-adultos" min="1" max="${
              familia.adultos
            }" class="swal2-input" style="width: 100px; margin-top: 5px;">
          </div>
          ${
            familia.ninos > 0
              ? `<div>
                   <label>Niños (máx ${familia.ninos}):</label>
                   <input type="number" id="swal-input-ninos" min="0" max="${familia.ninos}" class="swal2-input" style="width: 100px; margin-top: 5px;">
                 </div>`
              : ""
          }
        </div>
      `;

      const result = await Swal.fire({
        title: `Familia: ${familia.invitado}`,
        html: mensajeReservado + htmlForm,
        focusConfirm: false,
        preConfirm: () => {
          const asistentesAdultos =
            document.getElementById("swal-input-adultos").value;
          const asistentesNinos =
            familia.ninos > 0
              ? document.getElementById("swal-input-ninos").value
              : 0;

          if (asistentesAdultos > familia.adultos) {
            Swal.showValidationMessage(
              `No puede exceder de ${familia.adultos} adultos`
            );
            return false;
          }

          if (familia.ninos > 0 && asistentesNinos > familia.ninos) {
            Swal.showValidationMessage(
              `No puede exceder de ${familia.ninos} niños`
            );
            return false;
          }

          return { asistentesAdultos, asistentesNinos };
        },
        confirmButtonText: "Confirmar",
        showCancelButton: true,
      });

      if (result.isConfirmed) {
        const { asistentesAdultos, asistentesNinos } = result.value;

        // Confirmación de asistencia
        Swal.fire(
          `Asistencia confirmada para ${asistentesAdultos} adultos y ${asistentesNinos} niños`
        );

        // Enviar el correo de confirmación
        enviarCorreo(codigo, familia, asistentesAdultos, asistentesNinos);
      }
    } else {
      Swal.fire("Error", "Código no encontrado", "error");
    }
  }
};
</script>

<style>
.swal2-popup {
  border: 4px solid gold; /* Orilla dorada */
  background-color: #ffffff; /* Fondo rojo ocre */
}

/* Cambiar el estilo del título */
.swal2-title {
  color: goldenrod; /* Color del título */
  font-weight: bold;
}
.swal2-confirm {
  background-color: #a40505;
}
</style>
