<template>
  <q-form @submit="onSubmit" @reset="onReset" class="q-px-md">
    <div class="q-mb-md">
      <q-input
        dense
        v-model="article"
        label="Artículo"
        hint="Ingrese el artículo (PC, Impresora)"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor escriba un artículo',
        ]"
      />

      <q-input
        dense
        v-model="brand"
        label="Marca"
        hint="Ingrese la marca del dispositivo"
        lazy-rules
        :rules="[
          (val) =>
            (val && val.length > 0) || 'Ingrese la marca del dispositivo',
        ]"
      />

      <q-input
        dense
        v-model="serial"
        label="Serial"
        hint="Ingrese el serial del dispositivo"
      />
    </div>
    <div class="text-center q-pa-md">
      <q-btn no-caps label="Guardar" type="submit" color="primary" />
      <q-btn
        no-caps
        label="Limpiar"
        type="reset"
        color="primary"
        flat
        class="q-ml-sm"
      />
    </div>
  </q-form>
</template>

<script setup>
import { ref } from "vue";
import { api } from "boot/axios";
import { useQuasar } from "quasar";

const $q = useQuasar();

let article = ref(null);
let brand = ref(null);
let serial = ref(null);
let state = ref(0);

const emit = defineEmits(["refreshTable"]);

async function onSubmit() {
  await api.post("/device/register", {
    article: article.value,
    brand: brand.value,
    serial: serial.value ?? "",
    diagnostic: "",
    state: state.value,
    userId: 2,
  });
  $q.notify({
    message: "Dispositivo creado",
  });
  emit("refreshTable");
}

function onReset() {
  article.value = null;
  brand.value = null;
  serial.value = null;
  state.value = 0;
}
</script>
