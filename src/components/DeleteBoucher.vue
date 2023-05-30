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
import { useQuasar } from "quasar";

const $q = useQuasar();

const props = defineProps({ boucherId: Number });

const emit = defineEmits(["refreshTable"]);

async function onSubmit() {
  await api.post("/boucher/delete/" + props.boucherId);
  $q.notify({
    message: "Factura borrada",
  });
  emit("refreshTable");
}
</script>
