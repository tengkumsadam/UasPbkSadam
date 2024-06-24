<template>
  <q-layout view="hHh lpR fFf">
    <q-header>
      <q-toolbar>
        <q-btn flat label="Todos" @click="navigateTo('/todos')" />
        <q-btn flat label="Posts" @click="navigateTo('/posts')" />
        <q-btn flat label="Albums" @click="navigateTo('/albums')" />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { useRouter } from 'vue-router';

export default {
  name: 'App',
  setup() {
    const router = useRouter();

    const navigateTo = (path) => {
      router.push(path);
    };

    return {
      navigateTo,
    };
  },
};
</script>

<style lang="sass">
@import 'quasar/src/css/index.sass'
</style>