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
    reservado: `${familia.adultos + familia.ninos} personas`,
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

  // Convertir el código ingresado a mayúsculas
  const codigoMayusculas = codigo.toUpperCase();

  if (codigoMayusculas) {
    const familia = invitaciones.find((inv) => inv.codigo === codigoMayusculas);

    if (familia) {
      // Calcular el total de espacios reservados (adultos + niños)
      const totalReservado = familia.adultos + familia.ninos;

      let mensajeReservado = `Tienen reservados ${totalReservado} espacios:<br/> <br/>`;

      // Crear los campos de entrada para adultos y niños
      const htmlForm = `
        <div style="display: flex; flex-direction: column; align-items: center; gap: 10px;">
          <div>
            <label>Adultos: </label>
            <input type="text" id="swal-input-adultos" class="swal2-input" style="width: 100px; margin-top: 5px;" inputmode="numeric" pattern="[0-9]*">
          </div>
          <div>
            <label>Niños: </label>
            <input type="text" id="swal-input-ninos" class="swal2-input" style="width: 100px; margin-top: 5px;" inputmode="numeric" pattern="[0-9]*">
          </div>
        </div>
      `;

      const result = await Swal.fire({
        title: `${familia.invitado}`,
        html: mensajeReservado + htmlForm,
        focusConfirm: false,
        preConfirm: () => {
          // Tomar los valores de los campos, si están vacíos se asignan como 0
          const asistentesAdultos =
            document.getElementById("swal-input-adultos").value || "0";
          const asistentesNinos =
            document.getElementById("swal-input-ninos").value || "0";

          // Validar que solo se ingresen números enteros positivos
          if (
            !/^\d+$/.test(asistentesAdultos) ||
            !/^\d+$/.test(asistentesNinos)
          ) {
            Swal.showValidationMessage(
              "Por favor, ingrese solo números enteros positivos"
            );
            return false;
          }

          // Convertir los valores a enteros
          const asistentesAdultosInt = parseInt(asistentesAdultos);
          const asistentesNinosInt = parseInt(asistentesNinos);

          const totalAsistentes = asistentesAdultosInt + asistentesNinosInt;

          // Validar que al menos una persona asista
          if (totalAsistentes === 0) {
            Swal.showValidationMessage("Debe asistir al menos una persona");
            return false;
          }

          // Validar que no se deje ningún campo vacío
          if (asistentesAdultosInt === 0 && asistentesNinosInt === 0) {
            Swal.showValidationMessage(
              "Por favor, ingrese el número de asistentes adultos o niños"
            );
            return false;
          }

          // Validar que la suma no exceda el total reservado
          if (totalAsistentes > totalReservado) {
            Swal.showValidationMessage(
              `La suma de adultos y niños no puede exceder el total reservado de ${totalReservado} personas`
            );
            return false;
          }

          return { asistentesAdultosInt, asistentesNinosInt };
        },
        confirmButtonText: "Confirmar",
        showCancelButton: true,
      });

      if (result.isConfirmed) {
        const { asistentesAdultosInt, asistentesNinosInt } = result.value;

        // Confirmación de asistencia
        Swal.fire("Asistencia confirmada");

        // Enviar el correo de confirmación
        enviarCorreo(codigo, familia, asistentesAdultosInt, asistentesNinosInt);
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
