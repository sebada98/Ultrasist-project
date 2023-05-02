<template>
  <div class="content">
    <div class="container-fluid p-4 mx-3">
      <div class="card ancho forgot">
        <div class="card-header">
          <h1>Recuperación de clave</h1>
        </div>
        <div class="card-body">
          <p>Captura tu correo</p>
          <input type="text" v-model="email" />
          <br />
          <div class="row">
            <vue-recaptcha
              id="solvecaptcha"
              ref="recaptcha"
              sitekey="6LetXssSAAAAAF7WikI044mAdOiyyKPUxkklqMtN"
              @expired="onCaptchaExpired"
              @verify="onCaptchaVerified"
            />
          </div>
        </div>
        <div class="card-footer">
          <div class="d-flex justify-content-between">
            <button class="btn btn-success" @click="enviar">Enviar</button>
            <a class="btn btn-warning" href="/ui/login">Regresar a login</a>
          </div>
        </div>
      </div>
    </div>
    <MessageComponent
      ref="message01"
      alertType="3"
      duration="4000"
      :text="errorText"
      iconType="1"
      style="max-width: 600px"
    />
    <AvisoWindow
      ref="avisoWindow"
      avisoTitulo="Recuperación de clave"
      :avisoMsg="msg"
      :target="destino"
    />
    <FooterComponent />
  </div>
</template>

<script>
import axios from "axios";
import store from "@/store";
import { VueRecaptcha } from "vue-recaptcha";
import AvisoWindow from "@/components/AvisoWindow.vue";


const emailRegex = new RegExp(
  /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
);

const HTTP_STATUS = {
  OK: 200,
  FORBIDDEN: 403,
};

export default {
  data() {
    return {
      email: "",
      msg: "",
      destino: "",
      errorText: "",
    };
  },
  components: {
    VueRecaptcha,
    AvisoWindow,
  },
  mounted() {
    store.commit("setDestination", "/ui/results");
  },
  methods: {
    enviar: function () {
      if (!emailRegex.test(this.email)) {
        this.errorText = "Formato de correo inválido";
        this.$refs.message01.presenta();
        return;
      }
      axios
        .get("https://access.qbits.mx/api/forgot-password-request", {
          headers: { test: "ok" },
          params: { email: this.email },
        })
        .then((response) => {
          response;
          store.commit("setDestination", "/ui/forgot-confirm");
          this.msg = "Te hemos enviado un correo con un token de recuperación.";
          this.destino = "/ui/forgot-confirm";
          this.$refs.avisoWindow.openModal();
        })
        .catch((error) => {
          error;
          this.msg = error.response;
          this.destino = "";
          this.$refs.avisoWindow.openModal();
        });
    },
    onCaptchaVerified(recaptchaToken) {
      this.captcha = false;
      axios
        .post("https://access.qbits.mx/api/check-captcha", {
          response: recaptchaToken,
          ip: "127.0.0.1",
        })
        .then((response) => {
          if (response.status === HTTP_STATUS.OK) {
            this.captcha = true;
          } else {
            this.msgErr = "Regreso con un estatus: " + response.status;
          }
        })
        .catch((error) => {
          console.log(error.data);
          this.msgErr = "Ha ocurrido un error de red: " + error;
        })
        .finally(() => console.log("this.loading = false"));
    },
    onCaptchaExpired() {
      this.captcha = false;
      this.$refs.recaptcha.reset();
    },
  },
};
</script>

<style scoped>
.ancho {
  max-width: 600px;
  margin: auto;
  margin-top: 100px;
}
@media screen and (min-width: 768px) {
  .forgot{
    margin-top: -50px;
  }
}
</style>
