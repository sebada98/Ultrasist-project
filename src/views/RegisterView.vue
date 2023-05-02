<template>
  <div>
    <SideBarKixComponent/>
    <div class="container-fluid p-4 mx-3 login">
      <div class="ancho centra">
        <div class="register-card register">

          <div class="row">
            <div class="col-sm-12">
              <div class="card-header">
                <label class="register-label">Forma de registro</label>

              </div>

              <div class="card">
                <div class="card-body">
                  <div class="row">
                    <h1>Registro</h1>
                  </div>
                  <small class="register-label"
                    >Introduce los datos que se te solicitan</small
                  >
                  <br /><br />

                  <div class="form-group">
                    <div class="row px-3">
                      <input
                        type="text"
                        class="borders form-control icon-email"
                        placeholder="ejemplo@correo.com"
                        v-model="correo"
                      />
                      <small class="notValid">{{ msgCorreo }}</small>
                    </div>
                    <br />
                    <div class="row px-3">
                      <input
                        type="text"
                        class="borders form-control icon-user"
                        placeholder="tu nick"
                        v-model="nick"
                      />
                      <small class="notValid">{{ msgNick }}</small>
                    </div>
                    <br />
                    <div class="col form-group">
                      <div class="row px-3">
                        <input
                          :type="pwInputValue"
                          class="borders form-control icon-lock"
                          placeholder="contraseña"
                          v-model="password"
                        />
                        <small class="notValid">{{ msgPassword }}</small>
                      </div>
                      <div class="row px-3">
                        <a href="#" @click="oculta" class="a3">
                          <i
                            :class="[
                              pwVisible ? 'fas fa-eye-slash' : 'fas fa-eye',
                            ]"
                          ></i>
                        </a>
                      </div>
                    </div>
                  </div>

                  <br />
                  <div class="row">
                    <vue-recaptcha
                      id="solvecaptcha"
                      ref="recaptcha"
                      :rules="checkRules"
                      sitekey="6LetXssSAAAAAF7WikI044mAdOiyyKPUxkklqMtN"
                      @expired="onCaptchaExpired"
                      @verify="onCaptchaVerified"
                    />
                  </div>
                  <br />
                  <div class="form-group row text-center">
                    <div class="col">
                      <button
                        @click="register"
                        class="btn btn-success"
                        :disabled="incorrecto"
                      >
                        Solicitar Registro
                      </button>
                    </div>
                    <div class="col">
                      <button @click="openLoginPage" class="btn btn-warning">
                        Regresar a Login
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SideBarKixComponent from "@/components/SideBarKixComponent.vue";
import { VueRecaptcha } from "vue-recaptcha";
import { useToast } from "vue-toastification";
import router from "../router";
import axios from "axios";

const nickRegex = new RegExp(/^\w+$/);
const emailRegex = new RegExp(
  /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
);

const HTTP_STATUS = {
  OK: 200,
  FORBIDDEN: 403,
};

export default {
  setup() {
    // Get toast interface
    const toast = useToast();
    // Make it available inside methods
    return { toast };
  },
  data() {
    return {
      incorrecto: true,

      correo: "",
      msgCorreo: "",
      nick: "",
      msgNick: "",
      password: "",
      msgPassword: "",
      version: process.env.VUE_APP_VERSION,

      msgErr: "",
      captcha: false,
      checkRules: [(v) => !!v || ""],
      pwVisible: false,
      pwInputValue: "password",
    };
  },
  computed: {
    habilitaBoton() {
      return (
        !this.captcha ||
        this.msgCorreo.length > 0 ||
        this.msgNick.length > 0 ||
        this.msgPassword.length > 0
      );
    },
  },
  watch: {
    password(value) {
      this.msgPassword = "";
      if (value.length < 8) {
        this.msgPassword = "La clave debe tener 8 caracteres o mas";
      }
      if (value.length > 24) {
        this.msgPassword = "La clave debe tener 24 caracteres o menos";
      }
      if (value.replace(/[*a-z]/g, "") == value) {
        this.msgPassword = "La cadena debe posser al menos una minúscula";
      }
      if (value.replace(/[*A-Z]/g, "") == value) {
        this.msgPassword = "La cadena debe posser al menos una mayúscula";
      }
      if (value.replace(/[*0-9]/g, "") == value) {
        this.msgPassword = "La cadena debe posser al menos un número";
      }
      if (value.replace(/[&\\#, +(\-\\_)$~%.'":*?<>{}]/g, "") == value) {
        this.msgPassword = "La cadena debe posser al menos un símbolo";
      }
      this.habilitaSend();
    },
    correo() {
      this.msgCorreo = "";
      if (!emailRegex.test(this.correo)) {
        this.msgCorreo = "Formato de correo inválido";
      }
      if (this.correo.length < 7) {
        // a@x.com
        this.msgCorreo = "Longitud inválida de correo (mínimo 7 caracters)";
      }
      if (this.correo.length > 30) {
        this.msgCorreo = "Longitud inválida de correo (máximo 30 caracters)";
      }
      this.habilitaSend();
    },
    nick() {
      this.msgNick = "";
      if (
        this.nick.length < 4 ||
        this.nick.length > 16 ||
        !nickRegex.test(this.nick)
      ) {
        this.msgNick = "Emplear entre 4 y 16 caracters alfanuméricos";
      }
      this.habilitaSend();
    },
    captcha() {
      this.habilitaSend();
    },
  },
  components: {
    VueRecaptcha,
    SideBarKixComponent
  },
  methods: {
    habilitaSend() {
      var err =
        this.msgPassword.length > 0 ||
        this.msgCorreo.length > 0 ||
        this.msgNick.length > 0;
      var empty =
        this.password.length < 1 &&
        this.nick.length < 1 &&
        this.correo.length < 1;
      var other =
        this.captcha && this.correo.length < 30 && emailRegex.test(this.correo);
      this.incorrecto = empty || err || !other;
    },
    oculta() {
      this.pwVisible = !this.pwVisible;
      this.pwInputValue = this.pwVisible ? "text" : "password";
    },
    errorAlert(error) {
      this.toast.error(error, {
        position: "bottom-right",
        timeout: 5000,
        closeOnClick: true,
        pauseOnFocusLoss: true,
        pauseOnHover: true,
        draggable: true,
        draggablePercent: 0.6,
        showCloseButtonOnHover: false,
        hideProgressBar: false,
        closeButton: "button",
        icon: true,
        rtl: false,
      });
    },
    register() {
      axios
        .post("https://access.qbits.mx/api/register-request", {
          correo: this.correo,
          nick: this.nick,
          claveHash: this.password,
        })
        .then((response) => {
          console.log(response.data);
          router.push("/ui/registerconfirm");
        })
        .catch((error) => {
          if (error.response) {
            this.errorAlert(error.response);
          }
        });
    },
    openLoginPage() {
      router.push("/ui/login").catch(() => {});
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.register-card {
  animation: fadeIn 1s ease-in-out;
  background-color: rgba(10, 23, 38, 0.8);
  border-radius: 5px;
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.a2 {
  color: #006600;
  text-align: right;
  text-decoration: none;
}

.a2:hover {
  color: #009900;
}
.a3 {
  color: #fff;
  text-align: right;
  text-decoration: none;
}

.a3:hover {
  color: #fff;
}
.ancho {
  max-width: 360px;
}

.centra {
  margin: auto;
  padding-top: 10%;
}

.form-control:focus {
  box-shadow: inset 0 0 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(168, 161, 161, 0.58);
}

.icon-email {
  padding-left: 30px;
  background: url("@/assets/email.png") no-repeat left;
  background-size: 14px;
  background-position: 2%50%;
}

.icon-user {
  padding-left: 30px;
  background: url("@/assets/user.png") no-repeat left;
  background-size: 14px;
  background-position: 2%50%;
}

.icon-lock {
  padding-left: 30px;
  background: url("@/assets/lock.png") no-repeat left;
  background-size: 14px;
  background-position: 2%50%;
}

.notValid {
  color: #ff0000;
  text-align: right;
}

.fondo {
  position: absolute;
  background-size: cover;
  background-position: center center;
  width: 100%;
  height: 100%;
  z-index: -1;
  background-color: #fff;
}

.card {
  background-color: transparent;
}

.logo {
  width: 100px;
  height: 140px;
  margin-bottom: 30px;
}

h1,
small,
label {
  color: #fff;
}
button {
  transition: all 0.3s;
}

button:hover {
  transform: translateY(-3px);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
}

button:active {
  transform: translateY(1px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.15);
}


@media screen and (min-width: 768px) {
  .register{
    margin-top: -100px;
  }
}

.register-label {
  color: #fff;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
  font-weight: bold;
}

</style>

