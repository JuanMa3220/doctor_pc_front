<template>
  <q-form @submit="onSubmit" class="q-px-md">
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
      <q-input
        dense
        v-model="diagnostic"
        label="Disgnostico"
        hint="Ingrese el diagnostivo del dispositivo"
      />
      <q-select
        v-model="state"
        label="Estado"
        stack-label
        dense
        options-dense
        :options="stateOptions"
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
import { useQuasar } from "quasar";

const $q = useQuasar();

const props = defineProps({ deviceId: Number });

let article = ref(null);
let brand = ref(null);
let serial = ref(null);
let diagnostic = ref(null);
let state = ref();
let stateOptions = ref([
  {
    label: "Recibido",
    value: 0,
  },
  {
    label: "Revision",
    value: 1,
  },
  {
    label: "Listo",
    value: 2,
  },
]);

const emit = defineEmits(["refreshTable"]);

async function onSubmit() {
  await api.post("/device/edit/" + props.deviceId, {
    id: props.deviceId,
    article: article.value,
    brand: brand.value,
    serial: serial.value ?? "",
    diagnostic: diagnostic.value ?? "",
    state: state.value.value,
    userId: 2,
  });
  $q.notify("Dispositivo editado");
  emit("refreshTable");
}

onBeforeMount(async () => {
  let response = await api.get("/device/" + props.deviceId);
  article.value = response.data.article;
  brand.value = response.data.brand;
  serial.value = response.data.serial;
  diagnostic.value = response.data.diagnostic;
  state.value = response.data.state;
});
</script>
