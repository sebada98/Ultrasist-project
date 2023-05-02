<template>
    <div class="content">
        
        <div class="card ancho">
            <div class="card-header">
                <h1>Confirmación de recuperación</h1>
            </div>
            <div class="card-body">
                <div class="row">
                    <div class="col">
                        Para reestablecer tu clave, captura el token eniado a tu correo y un nuevo password
                    </div>
                </div>
                <div class="row">
                    <div class="col">
                        Token
                    </div>
                    <div class="col">
                        <input type="text" v-model="token" />
                    </div>                    
                </div>    
                <br/>  
                <div class="row">
                    <div class="col">
                        Password
                    </div>
                    <div class="col">
                        <input type="password" v-model="pass" />
                    </div>                    
                </div>                            
            </div>
            <div class="card-footer">
                <div class="d-flex justify-content-between">
                    <button class="btn btn-success" @click="enviar">Enviar</button>
                    <a class="btn btn-warning" href="/ui/login">Cancelar e ir a login</a>
                </div>
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
import AvisoWindow from '@/components/AvisoWindow.vue'
export default {
  components: {
    AvisoWindow
  },
    data() {
        return {
            token:'',
            msg:'',
            destino:'',
            pass:''
        }
    }, 
    methods: {
        processErrorResponse: function(error) {
            if(error.exceptionTypeNumber==1016) {
                var result = ''
                for (var i = 0; i < error.strengthViolations.length; i++) {
                    result += error.strengthViolations[i].message + ' \n '
                }
                return result;
            } else {
                return error.exceptionShortDescription
            }
        },
        enviar: function() {
            axios.post("https://access.qbits.mx/api/forgot-password-update", {
                token: this.token,
                pass: this.pass
            })
            .then(response => {
                response;
                store.commit('setDestination', '/ui/results')
                this.msg = 'Felicidades. Tu clave ha sido cambiada exitosamente.'
                this.destino = '/ui/login'
                this.$refs.avisoWindow.openModal()
            })
            .catch( error => {
                error;
                this.msg = this.processErrorResponse(error.response.data)
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
