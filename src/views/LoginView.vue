<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-md-3">
        <SideBarKixComponent />
      </div>
      <div class="col-md-9">
        <div class="card login-card">
        <div class="card login-card dark-card">
          <div class="card-body">
            <h1 class="card-title">Login</h1>
            <form @submit.prevent="checkCredentials">
              <div class="form-group">
                <label for="email">Email</label>
                <input
                  type="email"
                  class="form-control dark-input"
                  id="email"
                  v-model="fieldUser"
                  :class="{ 'is-invalid': usrErrorMsgClass }"
                  placeholder="example@mail.com"
                  required
                />
                <div class="invalid-feedback">
                  Invalid email format
                </div>
              </div>
              <div class="form-group">
                <label for="password">Password</label>
                <div class="input-group">
                  <input
                    type="password"
                    class="form-control dark-input"
                    id="password"
                    v-model="fieldPassword"
                    :class="{ 'is-invalid': passErrorMsgClass }"
                    placeholder="********"
                    required
                  />
                  <div class="input-group-append">
                    <button
                      class="btn btn-outline-secondary dark-outline-btn"
                      type="button"
                      @click="togglePasswordVisibility"
                    >
                      <i
                        :class="{ 'fa-eye': showPassword, 'fa-eye-slash': !showPassword }"
                      ></i>
                    </button>
                  </div>
                  <div class="invalid-feedback">
                    Password must be at least 5 characters long
                  </div>
                </div>
              </div>
              <div class="form-group">
                <button
                  type="submit"
                  class="btn btn-primary dark-btn"
                  :disabled="inhabilitado"
                >
                  Login
                </button>
                <router-link
                  to="/ui/register"
                  class="btn btn-outline-secondary dark-outline-btn"
                >
                  Register
                </router-link>
              </div>
              <div class="form-group">
                <router-link to="/ui/forgot" class="text-muted dark-link">
                  Forgot password?
                </router-link>
              </div>
            </form>
            <MessageComponent
              ref="message01"
              alertType="3"
              duration="4000"
              :text="errorText"
              iconType="1"
              style="max-width: 600px"
            />
          </div>
        </div>
      </div>
      </div>
    </div>
  </div>
</template>
<script>
import SideBarKixComponent from "@/components/SideBarKixComponent.vue";
import axios from "axios";
import store from "@/store";
import router from "@/router";

const apiEndpoint = "https://access.qbits.mx/api/login";

export default {
  components: {
    SideBarKixComponent,
  },
  data() {
    return {
      fieldUser: "",
      fieldPassword: "",
      usrErrorMsgClass: false,
      passErrorMsgClass: false,
      errorText: "",
      showPassword: false,
    };
  },
  computed: {
    inhabilitado() {
      return (
        !this.fieldUser ||
        !this.fieldPassword ||
        this.usrErrorMsgClass ||
        this.passErrorMsgClass
      );
    },
  },
  methods: {
    togglePasswordVisibility() {
      this.showPassword = !this.showPassword;
    },
    checkCredentials() {
      axios
        .post(apiEndpoint, {
          user: this.fieldUser,
          cred:       this.fieldPassword,
    })
    .then((response) => {
      const userData = response.data;
      store.commit("setUserData", userData);
      const target = this.detecta(store.state.userData.role);
      router.push(target);
    })
    .catch((error) => {
      this.errorText = error.response
        ? error.response.data.exceptionLongDescription
        : error;
      this.$refs.message01.presenta();
    });
},
detecta(roles) {
  if (store.state.destination.length > 0) {
    const target = store.state.destination;
    store.commit("setDestination", "");
    return target;
  }
  if (typeof roles === "string") {
    return "/";
  } else {
    for (let i = 0; i < roles.length; i++) {
      switch (roles[i].nombre) {
        case "admin":
          return "/ui/admin";
        case "user":
        case "normal":
          return "/";
        default:
          return "/";
      }
    }
  }
},
},
};
</script>

<style scoped>
.dark-card {
  background-color: #333;
  color: #fff;
  transition: all 0.3s;
}

.dark-card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  transform: translateY(-5px);
}

.dark-input {
  background-color: #555;
  color: #fff;
  border-color: #777;
}

.dark-input:focus {
  border-color: #ccc;
  box-shadow: 0 0 0 0.2rem rgba(255, 255, 255, 0.25);
}

.dark-btn {
  background-color: #007bff;
  color: #fff;
  border-color: #007bff;
}

.dark-btn:hover {
  background-color: #0069d9;
  border-color: #0062cc;
}

.dark-outline-btn {
  color: #fff;
  border-color: #fff;
}

.dark-outline-btn:hover {
  color: #333;
  background-color: #fff;
  border-color: #fff;
}

.dark-link {
  color: #ccc;
}

.dark-link:hover {
  color: #fff;
  text-decoration: underline;
}

.login-card-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

</style>
