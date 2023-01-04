<template>
    <v-form class="mt-10" @submit.prevent="handleSubmit">

        <v-text-field type="email" label="Correo electrónico" outlined v-model="email"></v-text-field>

        <v-text-field label="Contraseña" type="password" outlined v-model="password"></v-text-field>

        <v-alert type="error" v-if="isInvalidBackend">
            <div v-html="backendErrors">
            </div>
        </v-alert>

        <v-btn block color="primary" type="submit">
            Iniciar sesión
        </v-btn>

    </v-form>
</template>

<script>
import axios from 'axios'

export default {
    name: 'LoginForm',
    data() {
        return {
            email: '',
            password: '',
            isInvalidBackend: false,
            backendErrors: '',
        }
    },
    methods: {
        handleSubmit() {
            this.isInvalidBackend = false;
            this.backendErrors = ''

            axios.post('https://position-backend-production.up.railway.app/api/login', {
                email: this.email,
                password: this.password
            }).then(response => {
                const data = response.data

                localStorage.setItem('user', JSON.stringify(data.user))
                localStorage.setItem('token', data.token)

                this.$router.push('/mis-montos')
                // location.href = '/mis-montos'

            }).catch(error => {
                this.backendErrors = error.response.data.message

                this.isInvalidBackend = true
            });
        }
    }
}

</script>