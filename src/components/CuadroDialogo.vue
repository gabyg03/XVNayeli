<template>
  <q-dialog v-model="isOpen" persistent>
    <q-card>
      <q-card-section>
        <div class="text-h6">Confirmar Asistencia</div>
      </q-card-section>

      <q-card-section>
        <q-input v-model="codigo" label="Código de familia" />
        <p v-if="familyName">Familia: {{ familyName }}</p>
        <q-input
          v-model.number="numeroAsistentes"
          type="number"
          :max="maxAsistentes"
          label="Número de asistentes"
        />
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="Cancelar" color="primary" @click="closeDialog" />
        <q-btn
          flat
          label="Confirmar"
          color="primary"
          @click="confirmarAsistencia"
        />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  modelValue: Boolean,
});

const emit = defineEmits(["update:modelValue", "confirm"]);

const isOpen = ref(props.modelValue);
const codigo = ref("");
const familyName = ref("");
const numeroAsistentes = ref(1);
const maxAsistentes = ref(5);

watch(
  () => props.modelValue,
  (newVal) => {
    isOpen.value = newVal;
  }
);

const closeDialog = () => {
  emit("update:modelValue", false);
};

const confirmarAsistencia = () => {
  // Lógica para enviar la información a Google Sheets
  emit("confirm", {
    codigo: codigo.value,
    numeroAsistentes: numeroAsistentes.value,
  });
  closeDialog();
};

watch(isOpen, (newVal) => {
  if (!newVal) closeDialog();
});
</script>
