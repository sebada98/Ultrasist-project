<template>
    <div class="content">
        
        <div class="card ancho">
            <div class="card-header">
                <h1>Confirmación</h1>
            </div>
            <div class="card-body">
                <p>Para concluir tu registro, captura el token eniado a tu correo</p>
                <input type="text" v-model="token" />
            </div>
            <div class="card-footer">
                <button class="btn btn-success" @click="enviar">Enviar</button>
            </div>
        </div>

        <AvisoWindow 
            ref="avisoWindow"
            avisoTitulo="Comprobación del token" 
            :avisoMsg=msg
            :target=destino  />

        <FooterComponent class="down" />
    </div>
</template>

<script>
import axios from 'axios'
import store from '@/store'

export default {
    data() {
        return {
            token:'',
            msg:'',
            destino:''
        }
    }, 
    methods: {
        enviar: function() {
            axios.get("https://access.qbits.mx/api/register-confirm", {
                    headers: {test: 'ok'},
                    params: {token: this.token}
            })
            .then(response => {
                response;
                store.commit('setDestination', '/ui/results')
                this.msg = 'Felicidades. Registro Completo. Ahora ya puedes ingresaar al sistema con las credenciales que proporciionaste.'
                this.destino = '/ui/login'
                this.$refs.avisoWindow.openModal()
            })
            .catch( error => {
                error;
                this.msg = 'Token incorrecto. vuelve a intentar'
                this.destino = ''
                this.$refs.avisoWindow.openModal()
            })
        }
    }
}
</script>

<style scoped>
.ancho {
    max-width: 600px;
    margin: auto;
    margin-top: 100px;
}
</style>
