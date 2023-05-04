<template>
  <q-layout view="hHh lpR fFf">

    <q-header reveal elevated class="bg-primary text-white" height-hint="98">
      <q-toolbar>
        <q-toolbar-title>
          Doctor PC
        </q-toolbar-title>
        <q-btn flat icon-right="logout" to="/"/>
      </q-toolbar>

      <q-tabs align="left">
        <q-route-tab to="/dispositivos" label="Dispositivos" />
        <q-route-tab to="/facturas" label="Facturas" />
        <q-route-tab to="/admin" label="Administración" />
      </q-tabs>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>

<script setup>

</script>
