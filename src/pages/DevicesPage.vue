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
        :loading="loadingTable"
        style="height: 60vh"
        title="Aparatos"
        :rows="rows"
        :columns="columns"
        row-key="id"
      >
        <template v-slot:body="props">
          <q-tr :props="props">
            <q-td key="id" :props="props">
              {{ props.row.id }}
            </q-td>
            <q-td key="article" :props="props">
              {{ props.row.article }}
            </q-td>
            <q-td key="brand" :props="props">
              {{ props.row.brand }}
            </q-td>
            <q-td key="serial" :props="props">
              {{ props.row.serial }}
            </q-td>
            <q-td key="diagnostic" :props="props">
              {{ props.row.diagnostic }}
            </q-td>
            <q-td key="state" :props="props">
              {{ props.row.state }}
            </q-td>
            <q-td>
              <q-btn
                class="q-mx-sm"
                icon="edit"
                color="warning"
                rounded
                dense
                @click="editDevice(props.row.id)"
              ></q-btn>
              <q-btn
                class="q-mx-sm"
                icon="remove"
                color="negative"
                rounded
                dense
                @click="deleteDevice(props.row.id)"
              ></q-btn>
            </q-td>
          </q-tr>
        </template>
      </q-table>
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

  <q-dialog v-model="openEditDevice" persistent>
    <q-card>
      <q-card-section class="row items-center q-pb-none">
        <div class="text-h6">Edit Dispositivo</div>
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>
      <q-card-section>
        <EditDeviceForm
          style="width: 45rem; max-width: 95%"
          :device-id="editDeviceId"
          @refresh-table="refreshTable()"
        />
      </q-card-section>
    </q-card>
  </q-dialog>

  <q-dialog v-model="openDeleteDevice" persistent>
    <q-card>
      <q-card-section class="row items-center q-pb-none">
        <div class="text-h6">Eliminar Dispositivo</div>
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>
      <q-card-section>
        <DeleteDevice
          style="width: 45rem; max-width: 95%"
          :device-id="deleteDeviceId"
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
import EditDeviceForm from "src/components/EditDeviceForm.vue";
import DeleteDevice from "src/components/DeleteDevice.vue";

let rows = ref();
let columns = ref();
let openAddDevice = ref(false);
let openEditDevice = ref(false);
let openDeleteDevice = ref(false);
let loadingTable = ref(false);
let editDeviceId = ref(null);
let deleteDeviceId = ref(null);

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
  loadingTable.value = true;
  let response = await api.get("/devices/all");
  rows.value = response.data;
  openAddDevice.value = false;
  openEditDevice.value = false;
  openDeleteDevice.value = false;
  loadingTable.value = false;
}

function editDevice(deviceId) {
  editDeviceId.value = deviceId;
  openEditDevice.value = true;
}

function deleteDevice(deviceId) {
  deleteDeviceId.value = deviceId;
  openDeleteDevice.value = true;
}

onBeforeMount(() => {
  refreshTable();
});
</script>
