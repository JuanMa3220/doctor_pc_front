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
import { useQuasar } from "quasar";

const $q = useQuasar();

const props = defineProps({ deviceId: Number });

const emit = defineEmits(["refreshTable"]);

async function onSubmit() {
  await api.post("/device/delete/" + props.deviceId);
  $q.notify({
    message: "Dispositivo eliminado",
  });
  emit("refreshTable");
}
</script>
