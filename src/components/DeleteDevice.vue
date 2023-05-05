<template>
  <q-form @submit="onSubmit" class="q-px-md">
    Esta seguro que desea eliminar el dispositivo?
    <div class="text-center q-pa-md">
      <q-btn no-caps label="Eliminar" type="submit" color="negative" />
    </div>
  </q-form>
</template>

<script setup>
import { api } from "boot/axios";

const props = defineProps({ deviceId: Number });

const emit = defineEmits(["refreshTable"]);

async function onSubmit() {
  await api.post("/device/delete/" + props.deviceId);
  emit("refreshTable");
}
</script>
