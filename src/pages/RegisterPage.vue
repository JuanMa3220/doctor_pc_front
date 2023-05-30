<template>
  <div
    class="q-pa-md text-center q-mx-auto form-container"
    style="max-width: 40rem"
  >
    <h5 class="text-center">Registrate</h5>
    <q-card class="login-card">
      <q-card-section>
        <q-form @submit="onSubmit" @reset="onReset" class="q-px-md">
          <div class="q-py-md">
            <q-input
              dense
              v-model="document"
              label="Documento"
              hint="Ingrese su documento de identidad"
              lazy-rules
              :rules="[
                (val) => val || 'Por favor escribe tu documento de identidad',
                (val) =>
                  (val && val.length > 9 && val.length < 15) ||
                  'Debe ser mayor a 10 digitos y menor a 15',
              ]"
            />

            <q-input
              dense
              v-model="name"
              label="Nombre"
              hint="Ingresa tu nombre"
              lazy-rules
              :rules="[
                (val) =>
                  (val && val.length > 0) || 'Por favor escribe tu nombre',
              ]"
            />

            <q-input
              dense
              v-model="phone"
              label="Teléfono"
              hint="Ingrese su telefóno"
              lazy-rules
              :rules="[
                (val) =>
                  (val && val.length > 0) || 'Por favor escriba su telefono',
              ]"
            />

            <q-input
              dense
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
            <q-input
              dense
              type="password"
              v-model="confirmPassword"
              label="Repetir contraseña"
              hint="Repita su contraseña"
              lazy-rules
              :rules="[
                (val) =>
                  (val !== null && val !== '') ||
                  'Por favor escribe tu contraseña',
                (val) => val === password || 'Las contraseñas deben coincidir',
              ]"
            />
          </div>
          <div class="text-center q-pa-md">
            <q-btn no-caps label="Registrarse" type="submit" color="primary" />
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
        <q-btn
          no-caps
          flat
          label="Si ya tienes cuenta, inicia sesión aqui"
          to="/"
        ></q-btn>
      </q-card-actions>
    </q-card>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { api } from "boot/axios";
import { useRouter } from "vue-router";

let document = ref(null);
let name = ref(null);
let phone = ref(null);
let password = ref(null);
let confirmPassword = ref(null);

const router = useRouter();

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

<style lang="scss">
.form-container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
