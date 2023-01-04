<template>
    <v-btn color="secondary" @click="handleLogout">Cerrar sesión</v-btn>
</template>

<script>
import axios from 'axios'

export default {
    name: 'LogoutBtn',
    methods: {
        handleLogout() {
            axios.post('https://position-backend-production.up.railway.app/api/logout', {}, {
                headers: {
                    Authorization: 'Bearer ' + localStorage.getItem('token')
                }
            }).then(response => {

                if (response.data.message == 'Sesión cerrada') {
                    localStorage.clear()

                    // location.href = '/login';
                    this.$router.push('/login')
                }
            
            }).catch(error => {
                console.log(error)
            })
        }
    }
}
</script>