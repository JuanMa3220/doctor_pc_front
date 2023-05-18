<template>
  <div class="q-pa-md text-center q-mx-auto" style="max-width: 45rem">
    <div class="text-left">
      <q-btn
        no-caps
        label="Agregar"
        color="primary"
        @click="openAddBoucher = true"
      />
    </div>
    <div class="table-container q-py-md">
      <q-table
        :loading="loadingTable"
        style="height: 60vh"
        title="Facturas"
        :rows="rows"
        :columns="columns"
        row-key="id"
      >
        <template v-slot:body="props">
          <q-tr :props="props">
            <q-td key="id" :props="props">
              {{ props.row.id }}
            </q-td>
            <q-td key="checkin" :props="props">
              {{ props.row.checkin }}
            </q-td>
            <q-td key="checkout" :props="props">
              {{ props.row.checkout }}
            </q-td>
            <q-td key="total" :props="props">
              {{ props.row.total }}
            </q-td>
            <q-td key="state" :props="props">
              {{ props.row.state }}
            </q-td>
            <q-td key="description" :props="props">
              {{ props.row.description }}
            </q-td>
            <q-td>
              <q-btn
                class="q-mx-sm"
                icon="edit"
                color="warning"
                rounded
                dense
                @click="editBoucher(props.row.id)"
              ></q-btn>
              <q-btn
                class="q-mx-sm"
                icon="remove"
                color="negative"
                rounded
                dense
                @click="deleteBoucher(props.row.id)"
              ></q-btn>
            </q-td>
          </q-tr>
        </template>
      </q-table>
    </div>
  </div>
  <q-dialog v-model="openAddBoucher" persistent>
    <q-card>
      <q-card-section class="row items-center q-pb-none">
        <div class="text-h6">Agregar Factura</div>
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>
      <q-card-section>
        <AddBoucherForm
          style="width: 45rem; max-width: 95%"
          @refresh-table="refreshTable()"
        />
      </q-card-section>
    </q-card>
  </q-dialog>

  <q-dialog v-model="openEditBoucher" persistent>
    <q-card>
      <q-card-section class="row items-center q-pb-none">
        <div class="text-h6">Editar Factura</div>
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>
      <q-card-section>
        <EditBoucherForm
          style="width: 45rem; max-width: 95%"
          :boucher-id="editBoucherId"
          @refresh-table="refreshTable()"
        />
      </q-card-section>
    </q-card>
  </q-dialog>

  <q-dialog v-model="openDeleteBoucher" persistent>
    <q-card>
      <q-card-section class="row items-center q-pb-none">
        <div class="text-h6">Eliminar Factura</div>
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>
      <q-card-section>
        <DeleteBoucher
          style="width: 45rem; max-width: 95%"
          :boucher-id="deleteBoucherId"
          @refresh-table="refreshTable()"
        />
      </q-card-section>
    </q-card>
  </q-dialog>
</template>

<script setup>
import { ref, onBeforeMount } from "vue";
import { api } from "boot/axios";

import AddBoucherForm from "src/components/AddBoucherForm.vue";
import EditBoucherForm from "src/components/EditBoucherForm.vue";
import DeleteBoucher from "src/components/DeleteBoucher.vue";

let rows = ref();
let columns = ref();
let openAddBoucher = ref(false);
let openEditBoucher = ref(false);
let openDeleteBoucher = ref(false);
let loadingTable = ref(false);
let editBoucherId = ref(null);
let deleteBoucherId = ref(null);

columns.value = [
  {
    name: "checkin",
    label: "Fecha de Ingreso",
    field: (row) => row.checkin,
  },
  {
    name: "checkout",
    label: "Fecha de Salida",
    field: (row) => row.checkout,
  },
  {
    name: "total",
    label: "Taltal Factura",
    field: (row) => row.total,
  },
  {
    name: "state",
    label: "Estado",
    field: (row) => row.state,
    format: (state) => getState(state),
  },
  {
    name: "description",
    label: "Descripcion",
    field: (row) => row.description,
  },
];

function getState(state) {
  switch (state) {
    case 0:
      return "Pago";
    case 1:
      return "No pago";
    default:
      return "Otro";
  }
}

async function refreshTable() {
  loadingTable.value = true;
  let response = await api.get("/boucher/all");
  rows.value = response.data;
  openAddBoucher.value = false;
  openEditBoucher.value = false;
  openDeleteBoucher.value = false;
  loadingTable.value = false;
}

function editBoucher(boucherId) {
  editBoucherId.value = boucherId;
  openEditBoucher.value = true;
}

function deleteBoucher(boucherId) {
  deleteBoucherId.value = boucherId;
  openDeleteBoucher.value = true;
}

onBeforeMount(() => {
  refreshTable();
});
</script>
