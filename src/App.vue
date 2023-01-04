<template>
  <v-app>

    <v-main>
      <v-app-bar v-if="isLoggedIn" dense>
        <v-toolbar-title>
          Bienvenido: {{ user.name }}
        </v-toolbar-title>

        <v-spacer></v-spacer>

        <LogoutBtn />
      </v-app-bar>

      <v-container>
        <router-view />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import LogoutBtn from '@/components/LogoutBtn.vue'

export default {
  name: 'App',
  components: {
    LogoutBtn
  },
  data() {
    return {
      isLoggedIn: localStorage.getItem('user') != null && localStorage.getItem('token') != null,
      user: JSON.parse(localStorage.getItem('user')),
      token: localStorage.getItem('token'),
    }
  }
};
</script>
