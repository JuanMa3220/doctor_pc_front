<template>
  <q-form @submit="onSubmit" class="q-px-md">
    Esta seguro que desea eliminar la Factura?
    <div class="text-center q-pa-md">
      <q-btn no-caps label="Eliminar" type="submit" color="negative" />
    </div>
  </q-form>
</template>

<script setup>
import { api } from "boot/axios";

const props = defineProps({ boucherId: Number });

const emit = defineEmits(["refreshTable"]);

async function onSubmit() {
  await api.post("/boucher/delete/" + props.boucherId);
  emit("refreshTable");
}
</script>
