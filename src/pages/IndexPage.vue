<template>
  <div
    class="q-pa-md text-center q-mx-auto form-container"
    style="max-width: 40rem"
  >
    Index works!
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
