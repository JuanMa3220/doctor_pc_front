<template>
  <q-form @submit="onSubmit" @reset="onReset" class="q-px-md">
    <div class="q-mb-md">
      <q-input
        type="date"
        dense
        v-model="checkin"
        label="Fecha de Ingreso"
        hint="Ingrese la Fecha de entrada separada por /"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor escriba una Fecha',
        ]"
      />

      <q-input
        type="date"
        dense
        v-model="checkout"
        label="Fecha de Salida"
        hint="Ingrese la Fecha de salida separada por /"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor escriba una Fecha',
        ]"
      />

      <q-input
        type="number"
        dense
        v-model="total"
        label="Total"
        hint="Ingrese el total de la factura"
        lazy-rules
        :rules="[(val) => (val && val >= 0) || 'Por favor escriba el valor']"
      />

      <q-input
        dense
        v-model="description"
        label="Descripcion"
        hint="Ingrese la descripcion"
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

let checkin = ref(null);
let checkout = ref(null);
let total = ref(null);
let state = ref(0);
let description = ref(null);

const $q = useQuasar();

const emit = defineEmits(["refreshTable"]);

async function onSubmit() {
  await api.post("/boucher/register", {
    checkin: checkin.value,
    checkout: checkout.value,
    total: total.value ?? "",
    state: state.value,
    description: description.value,
  });
  $q.notify({
    message: "Factura creada",
  });
  emit("refreshTable");
}

function onReset() {
  checkin.value = null;
  checkout.value = null;
  total.value = null;
  state.value = 0;
  description.value = null;
}
</script>
