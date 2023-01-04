<template>
    <v-row justify="center">
        <v-col md="6" cols="12" align-self="center">
            <h1>Registrarme</h1>
            <hr>

            <RegisterForm />

        </v-col>

        <v-col cols="12" class="d-flex align-center">
            <h4 class="mx-auto">
                ¿Ya tienes una cuenta? Inicia sesión <router-link to="/login">aquí</router-link>
            </h4>
        </v-col>

    </v-row>
</template>

<script>
import RegisterForm from '@/components/RegisterForm.vue'

export default {
    name: "RegisterView",
    data() {
        return {
            isLoggedIn: localStorage.getItem('user') != null && localStorage.getItem('token') != null
        }
    },
    components: {
        RegisterForm
    },
    created() {
        if (this.isLoggedIn) {
            location.href = 'mis-montos'
            // this.$router.push('/mis-montos');
        }
    }
}

</script>