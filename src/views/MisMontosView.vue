<template>
    <v-row class="">
        <v-col md="3" cols="12">
            <NuevoMontoForm />
        </v-col>

        <v-col md="9" cols="12">
            <MontosGrafico @reRenderGrafico="$forceUpdate" />
        </v-col>

    </v-row>
</template>

<script>
import NuevoMontoForm from '@/components/NuevoMontoForm.vue';
import MontosGrafico from '@/components/MontosGrafico.vue'

export default {
    name: 'MisMontosView',
    components: {
        MontosGrafico,
        NuevoMontoForm
    },
    data() {
        return {
            isLoggedIn: localStorage.getItem('user') != null && localStorage.getItem('token') != null
        }
    },
    created() {
        if (!this.isLoggedIn) {
            location.href = '/'
            // this.$router.push('/login');
        }
    }
}
</script>