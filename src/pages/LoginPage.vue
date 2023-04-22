<template>
  <div class="q-pa-md text-center q-mx-auto" style="max-width: 40rem">
    <h5 class="text-center">Iniciar Sesion</h5>
    <q-card class="login-card">
      <q-card-section>
        <q-form @submit="onSubmit" @reset="onReset" class="q-px-md">
          <div class="q-py-md">
            <q-input
              v-model="document"
              label="Documento"
              hint="Ingrese su documento de identidad"
              lazy-rules
              :rules="[
                (val) =>
                  (val && val.length > 0) ||
                  'Por favor escribe tu documento de identidad',
              ]"
            />

            <q-input
              type="password"
              v-model="password"
              label="Contraseña"
              hint="Ingrese su contraseña"
              lazy-rules
              :rules="[
                (val) =>
                  (val !== null && val !== '') ||
                  'Por favor escribe tu contraseña',
              ]"
            />
          </div>
          <div class="text-center q-pa-md">
            <q-btn
              no-caps
              label="Iniciar Sesion"
              type="submit"
              color="primary"
            />
            <q-btn
              no-caps
              label="Limpiar"
              type="reset"
              color="primary"
              flat
              class="q-ml-sm"
            />
          </div>
        </q-form>
      </q-card-section>

      <q-separator />

      <q-card-actions vertical>
        <q-btn no-caps flat label="Registrate aqui si no tienes una cuenta" to="/register"></q-btn>
      </q-card-actions>
    </q-card>
  </div>
</template>

<script setup>
import { ref } from 'vue';

  let document = ref(null);
  let password = ref(null);

  function onSubmit() {
    console.log('Iniciar sesion en el backend');
  }

  function onReset() {
    document.value = null;
    password.value = null;
  }

</script>
