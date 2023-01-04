<template>
    <v-form @submit.prevent="handleSubmit" ref="form">
        <v-date-picker full-width v-model="fecha" locale="es-cl"></v-date-picker>

        <v-text-field type="number" label="Monto" v-model="monto" class="mt-3"></v-text-field>

        <v-alert type="error" class="my-3" v-if="isInvalidBackend">
            <div v-html="backendErrors"></div>
        </v-alert>

        <v-btn block color="primary" type="submit">Agregar monto</v-btn>

    </v-form>
</template>

<script>
import axios from 'axios'

export default {
    name: 'NuevoMontoForm',
    data() {
        return {
            fecha: '',
            monto: 0,
            isInvalidBackend: false,
            backendErrors: ''
        }
    },
    methods: {
        emitReRender() {
            this.$root.$emit('re-render-grafico')
        },
        handleSubmit() {

            this.isInvalidBackend = false
            this.backendErrors = ''

            let fecha = new Date(this.fecha);
            fecha.setDate(fecha.getDate() + 2);

            axios.post('https://position-backend-production.up.railway.app/api/ingresar-monto', {
                fecha: fecha.toLocaleString({
                    year: 'numeric',
                    month: 'numeric',
                    day: 'numeric',
                }),
                monto: this.monto
            }, {
                headers: {
                    Authorization: 'Bearer ' + localStorage.getItem('token')
                },
            }).then(response => {

                if (response.status == 200) {
                    this.$refs.form.reset()
                    this.fecha = ''

                    this.$root.$emit('re-render-grafico')
                }

            }).catch(error => {
                const errors = error.response.data.errors
                
                Object.values(errors).map(error => {
                    this.backendErrors += `${error[0]} <br>`
                })

                this.isInvalidBackend = true
            })
        }
    }
}
</script>