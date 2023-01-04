<template>
    <v-row justify="center">
        <v-col md="6" cols="12" align-self="center">
            <h1>Iniciar sesión</h1>
            <hr>

            <LoginForm />

        </v-col>

        <v-col cols="12" class="d-flex align-center">
            <h4 class="mx-auto">
                ¿No tienes una cuenta? Regístrate <router-link to="/register">aquí</router-link>
            </h4>
        </v-col>

    </v-row>
</template>

<script>
import LoginForm from '@/components/LoginForm.vue'

export default {
    name: "LoginView",
    data() {
        return {
            isLoggedIn: localStorage.getItem('user') != null && localStorage.getItem('token') != null
        }
    },
    components: {
        LoginForm
    },
    created() {
        if (this.isLoggedIn) {
            location.href = 'mis-montos'
            // this.$router.push('/mis-montos');
        }
    }
}

</script>