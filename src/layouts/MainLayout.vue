<template>
  <q-layout view="hHh lpR fFf">
    <q-header reveal elevated class="bg-primary text-white" height-hint="98">
      <q-toolbar>
        <q-toolbar-title> Doctor PC </q-toolbar-title>
        <q-btn flat icon-right="logout" to="/" />
      </q-toolbar>

      <q-tabs align="left">
        <q-route-tab to="/dispositivos" label="Dispositivos" />
        <q-route-tab to="/facturas" label="Facturas" />
        <q-route-tab label="Administración" @click="showAdminWarning = true" />
      </q-tabs>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>

  <q-dialog v-model="showAdminWarning">
    <q-card>
      <q-card-section>
        <div class="text-h6">Alert</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        Esta sección se encuentra en construcción.
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="OK" color="primary" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script setup>
import { ref } from "vue";
let showAdminWarning = ref(false);
</script>
