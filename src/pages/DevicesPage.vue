<template>
  <div class="q-pa-md text-center q-mx-auto" style="max-width: 45rem">
    <div class="text-left">
      <q-btn
        no-caps
        label="Agregar"
        color="primary"
        @click="openAddDevice = true"
      />
    </div>
    <div class="table-container q-py-md">
      <q-table
        title="Dispositivos"
        :rows="rows"
        :columns="columns"
        row-key="id"
      />
    </div>
  </div>
  <q-dialog v-model="openAddDevice" persistent>
    <q-card>
      <q-card-section class="row items-center q-pb-none">
        <div class="text-h6">Agregar Dispositivo</div>
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>
      <q-card-section>
        <AddDeviceForm
          style="width: 45rem; max-width: 95%"
          @refresh-table="refreshTable()"
        />
      </q-card-section>
    </q-card>
  </q-dialog>
</template>

<script setup>
import { ref, onBeforeMount } from "vue";
import { api } from "boot/axios";

import AddDeviceForm from "src/components/AddDeviceForm.vue";

let rows = ref();
let columns = ref();
let openAddDevice = ref(false);

columns.value = [
  {
    name: "article",
    label: "Artículo",
    field: (row) => row.article,
  },
  {
    name: "brand",
    label: "Marca",
    field: (row) => row.brand,
  },
  {
    name: "serial",
    label: "Serial",
    field: (row) => row.serial,
  },
  {
    name: "diagnostic",
    label: "Diagnostico",
    field: (row) => row.diagnostic,
  },
  {
    name: "state",
    label: "Estado",
    field: (row) => row.state,
    format: (state) => getState(state),
  },
];

function getState(state) {
  switch (state) {
    case 0:
      return "Recibido";
    case 1:
      return "Revision";
    default:
      return "Listo";
  }
}

async function refreshTable() {
  console.log("Refresh");
  let response = await api.get("/devices/all");
  rows.value = response.data;
  openAddDevice.value = false;
}

onBeforeMount(() => {
  refreshTable();
});
</script>
