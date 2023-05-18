<template>
  <q-form @submit="onSubmit" class="q-px-md">
    <div class="q-mb-md">
      <q-input
        dense
        v-model="checkin"
        label="Fecha de Ingreso"
        hint="Ingrese la Fecha de entrada"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor escriba una Fecha',
        ]"
      />

      <q-input
        dense
        v-model="checkout"
        label="Fecha de Salida"
        hint="Ingrese la Fecha de salida"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor escriba una Fecha',
        ]"
      />

      <q-input
        dense
        v-model="total"
        label="Total"
        hint="Ingrese el total de la factura"
        lazy-rules
        :rules="[
          (val) => (val && val.length > 0) || 'Por favor escriba el valor',
        ]"
      />

      <q-select
        v-model="state"
        label="Estado"
        stack-label
        dense
        options-dense
        :options="stateOptions"
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
    </div>
  </q-form>
</template>

<script setup>
import { ref, onBeforeMount } from "vue";
import { api } from "boot/axios";

const props = defineProps({ boucherId: Number });

let checkin = ref(null);
let checkout = ref(null);
let total = ref(null);
let state = ref();
let description = ref(null);
let stateOptions = ref([
  {
    label: "Pago",
    value: 0,
  },
  {
    label: "No Pago",
    value: 1,
  },
  {
    label: "Otro",
    value: 2,
  },
]);

const emit = defineEmits(["refreshTable"]);

async function onSubmit() {
  await api.post("/boucher/edit/" + props.boucherId, {
    id: props.boucherId,
    checkin: checkin.value,
    checkout: checkout.value,
    total: total.value ?? "",
    state: state.value.value,
    description: description.value,
  });
  emit("refreshTable");
}

onBeforeMount(async () => {
  let response = await api.get("/boucher/" + props.boucherId);
  console.log(response);
  checkin.value = response.data.checkin;
  checkout.value = response.data.checkout;
  total.value = response.data.total;
  state.value = response.data.state;
  description.value = response.data.description;
});
</script>
