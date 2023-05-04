<template>
  <div
    class="q-pa-md text-center q-mx-auto"
    style="max-width: 45rem;">
    <div class="text-left">
      <q-btn no-caps label="Agregar" color="primary"/>
    </div>
    <div class="table-container">
      <q-table
        title="Dispositivos"
        :rows="rows"
        :columns="columns"
        row-key="id" />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { api } from "boot/axios";
import { useRouter } from "vue-router";

let rows = ref();
let columns = ref();

const router = useRouter();

columns.value = [{
  name: 'article',
  label: 'Artículo',
  field: row => row.article
  },
  {
  name: 'brand',
  label: 'Marca',
  field: row => row.brand
  },
  {
  name: 'serial',
  label: 'Serial',
  field: row => row.serial
  },
  {
  name: 'diagnostic',
  label: 'Diagnostico',
  field: row => row.diagnostic
  },
  {
  name: 'state',
  label: 'Estado',
  field: row => row.state
  },
];

let device = {
  id: Math.random(),
  article: 'PC',
  brand: 'Lenovo',
  serial: 'SN123',
  diagnostic: 'No tiene arreglo',
  state: 2
}

rows.value = [
  device,
  device,
  device,
  device,
  device,
  device,
  device,
  device,
  device,
  device,
  device,
  device,
  device,
  device,
]

async function onSubmit() {
  let response = await api.post("/register", {
    document: document.value,
    name: name.value,
    phone: phone.value,
    password: password.value,
  });

  router.push("/home");
}

function onReset() {
  document.value = null;
  name.value = null;
  phone.value = null;
  confirmPassword.value = null;
  password.value = null;
}

</script>
