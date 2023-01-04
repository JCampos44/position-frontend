<template>
    <v-row class="">
        <v-col cols="3">
            <NuevoMontoForm />
        </v-col>

        <v-col cols="9">
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
            this.$router.push('/login');
        }
    }
}
</script>