<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" app>
      <v-list>
        <v-list-item>
          <v-list-item-title class="title">Postify</v-list-item-title>
        </v-list-item>
        <v-list-item link to="/">
          <v-list-item-icon><v-icon>mdi-home</v-icon></v-list-item-icon>
          <v-list-item-content><v-list-item-title>Home</v-list-item-title></v-list-item-content>
        </v-list-item>
        <v-list-item link to="/collections">
          <v-list-item-icon><v-icon>mdi-folder</v-icon></v-list-item-icon>
          <v-list-item-content><v-list-item-title>Collections</v-list-item-title></v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Postify</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    drawer: false,
  }),
};
</script>

<style>
.title {
  font-size: 1.5rem;
  font-weight: bold;
}
</style>