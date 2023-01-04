<template>
    <v-form class="mt-10" @submit.prevent="handleSubmit">

        <v-text-field label="Nombre" outlined v-model="name"></v-text-field>

        <v-text-field type="email" label="Correo electrónico" outlined v-model="email"></v-text-field>

        <v-text-field label="Contraseña" type="password" outlined v-model="password"></v-text-field>

        <v-text-field label="Confirmar contraseña" type="password" outlined v-model="password_confirmation"></v-text-field>

        <v-alert type="error" v-if="isInvalidBackend">
            <div v-html="backendErrors">
            </div>
        </v-alert>

        <v-btn block color="primary" type="submit">
            Registrarme
        </v-btn>

    </v-form>
</template>

<script>
import axios from 'axios'

export default {
    name: 'RegisterForm',
    data() {
        return {
            name: '',
            email: '',
            password: '',
            password_confirmation: '',
            isInvalidBackend: false,
            backendErrors: ''
        }
    },
    methods: {
        handleSubmit() {
            this.isInvalidBackend = false;
            this.backendErrors = ''

            axios.post('https://position-backend-production.up.railway.app/api/register', {
                name: this.name,
                email: this.email,
                password: this.password,
                password_confirmation: this.password_confirmation
            }).then(response => {
                const data = response.data

                localStorage.setItem('user', data.user)
                localStorage.setItem('token', data.token)

            }).catch(error => {
                const errors = error.response.data.errors
                
                Object.values(errors).map(error => {
                    this.backendErrors += `${error[0]} <br>`
                })

                this.isInvalidBackend = true

            });
        }
    }
}

</script>