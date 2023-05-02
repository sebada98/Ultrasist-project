<template>
  <div>
    <header>
      <div class="icon__menu">
        <i
          class="fa-solid fa-bars barras"
          id="btn_open"
          @mouseenter="abre"
          @mouseleave="cierra"
          :style="{ width: sidebarWidth3 }"
        ></i>
      </div>
      <div class="d-flex w-100 justify-content-end">
        <span v-if="idUser > 0" class="logeado"
          ><a href="" class="btn btn-light" v-on:click="handleLogout"
            >Logout</a
          ></span
        >
        <span v-else class="logear">
          <a href="/ui/login" class="btn btn-light">Login</a>
          <a href="/ui/register" class="btn btn-light">Register</a>
        </span>
      </div>
    </header>
    <!-- menu que solo se ve en desktop y tablets -->
    <div
      class="menu__side"
      id="menu_side"
      @mouseenter="abre"
      @mouseleave="cierra"
      :style="{ width: sidebarWidth }"
    >
      <div class="name__page">
        <!-- aqui iria el logo -->
        <img
          src="../assets/chambeaya.png"
          style="width: 35px; height: 35px"
          alt=""
        />
        <i class="fa-solid fa-user-alien"></i>
        <h4>ChambeaYa!</h4>
      </div>
      <!-- menu de opciones -->
      <div class="options__menu">
        <!-- aqui van los links -->
        <a href="/ui/home">
          <div class="option">
            
            <i class="fa-solid fa-house" title="Home"></i>
            <h4>Home</h4>
          </div>
        </a>
        <a href="/ui/datos">
          <div class="option">
            <!-- aqui va el icono : ejemplo el del home  le colocamos title:home -->
            <i class="fa-solid fa-user" title="Perfil"></i>
            <h4>Perfil</h4>
          </div>
        </a>
        <a href="/ui/ine">
          <div class="option">
            <!-- aqui va el icono : ejemplo el del home  le colocamos title:home -->
            <i class="fa-solid fa-address-card"></i>
            <h4>Ine</h4>
          </div>
        </a>
        <a href="/ui/anuncio">
          <div class="option">
            <!-- aqui va el icono : ejemplo el del home  le colocamos title:home -->
            <i class="fa-solid fa-rectangle-ad"></i>
            <h4>Anuncio</h4>
          </div>
        </a>
        <a href="/ui/pago">
          <div class="option">
            <!-- aqui va el icono : ejemplo el del home  le colocamos title:home -->
            <i class="fa-regular fa-credit-card"></i>
            <h4>Pago</h4>
          </div>
        </a>
        <a href="" v-on:click="handleLogout">
          <div class="option">
            <!-- aqui va el icono : ejemplo el del logout  le colocamos title:home -->
            <h4>Logout</h4>
          </div>
        </a>
        <a href="/ui/factura">
          <div class="option">
            <!-- aqui va el icono : ejemplo el del logout  le colocamos title:home -->
            <i class="fa-sharp fa-solid fa-money-bill" title="Facturacion"></i>
            <h4>Facturacion</h4>
          </div>
        </a>
        <a href="/ui/menu">
          
        </a>
      </div>
    </div>
    
  </div>
</template>

<script>
import { ref } from "vue";
import { computed } from "vue";
import { mapState, mapMutations } from "vuex";
import store from "@/store";
let collapsed = ref(true);
let collapsed3 = ref(true);
console.log(collapsed.value);

const toggleSidebar = () => (collapsed.value = !collapsed.value);
const abre = () => (collapsed.value = false);
const cierra = () => (collapsed.value = true);
const toggleSidebar3 = () => (collapsed3.value = !collapsed.value);
const abre3 = () => (collapsed3.value = false);
const cierra3 = () => (collapsed3.value = true);
const SIDEBAR_WIDTH = 250;
const SIDEBAR_WIDTH_COLLAPSED = 80;
let sidebarWidth = computed(
  () => `${collapsed.value ? SIDEBAR_WIDTH_COLLAPSED : SIDEBAR_WIDTH}px`
);
// en version mobile
const SIDEBAR_WIDTH2 = 250;
const SIDEBAR_WIDTH_COLLAPSED2 = 0;
let sidebarWidth3 = computed(
  () => `${collapsed.value ? SIDEBAR_WIDTH_COLLAPSED2 : SIDEBAR_WIDTH2}px`
);

export default {
  setup() {
    return {
      collapsed,
      collapsed3,
      toggleSidebar,
      toggleSidebar3,
      sidebarWidth,
      sidebarWidth3,
      abre,
      cierra,
      abre3,
      cierra3,
    };
  },
  data() {
    return {
      sidebarWidth2: "250px",
      sidebarWidth4: "250px",
      idUser: store.state.userData.idUser,
    };
  },
  computed: {
    ...mapState({
      nick: (state) => state.userData.nick,
    }),
  },
  methods: {
    collapsed2: function () {
      return collapsed.value
        ? (this.sidebarWidth2 = "80px")
        : this.sidebarWidth2;
    },
    collapsed4: function () {
      return collapsed3.value
        ? (this.sidebarWidth4 = "0px")
        : this.sidebarWidth4;
    },
    ...mapMutations({
      clearUserData: "clearUserData",
    }),
    handleLogout() {
      this.clearUserData();
      localStorage.clear();
      this.$router.push({ name: "login" });
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

a {
  text-decoration: none;
}

body {
  margin-top: 80px;
  margin-left: 80px;
  transition: margin-left 300ms cubic-bezier(0.785, 0.135, 0.15, 0.86);
}

header {
  width: 100%;
  height: 80px;
  background: rgba(0, 0, 0, 0.95);
  -webkit-backdrop-filter: blur(2px);
  backdrop-filter: blur(2px);
  border: 5px solid rgba(0, 0, 0, 0.325);
  border-left: 0px;
  display: flex;
  align-items: center;
  position: fixed;
  top: 0;
  right: 0;
  z-index: 200;
  color: #fff;
}

.icon__menu {
  width: 50px;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  transform: translateX(10px);
}

.icon__menu i {
  font-size: 20px;
  cursor: pointer;
  position: absolute;
}

.menu__side {
  width: 80px;
  height: 100%;

  background: HEX #0b263d;
  -webkit-backdrop-filter: blur(2px);
  backdrop-filter: blur(2px);
  border: 1px solid rgba(0, 0, 0, 0.25);
  position: fixed;
  top: 0;
  left: 0;
  color: #fff;
  font-size: 18px;
  z-index: 300;
  overflow: hidden;
  overflow-y: scroll;
  border-right: 10px solid rgba(0, 0, 0, 0.5);
  transition: all 300ms cubic-bezier(0.785, 0.135, 0.15, 0.86);
}

.logear {
  display: flex;
  width: 25%;
  gap: 3rem;
}

.logeado {
  display: flex;
  width: 25%;
  grid-area: 3rem;
}

/* hide scrollbar */
.menu__side::-webkit-scrollbar {
  display: none;
}

.menu__side {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.name__page {
  padding: 20px 30px;
  display: flex;
  align-items: center;
  margin-top: 10px;
}

.name__page i {
  width: 20px;
  margin-right: 20px;
}

.options__menu {
  padding: 20px 30px;
  position: absolute;
  top: 80px;
}

.options__menu a {
  color: #ddd;
  cursor: default;
  display: block;
  position: relative;
  transition: all 300ms;
}

.options__menu a:hover {
  color: #fff;
}

.options__menu .option {
  padding: 20px 0;
  display: flex;
  align-items: center;
  position: relative;
}

.options__menu .option i {
  width: 20px;
  margin-right: 20px;
  cursor: pointer;
}

.options__menu .option h4 {
  font-weight: 300;
  cursor: pointer;
}

a.selected {
  color: #fff;
}

a.selected::before {
  content: "";
  width: 3px;
  height: 80%;
  background-color: #fff;
  position: absolute;
  top: 10%;
  left: -30px;
}

/* use in js action */

.body_move {
  margin-left: 250px;
}

.menu__side_move {
  width: 250px;
}

@media screen and (max-width: 768px) {
  body {
    margin-left: -20px;
  }
  .menu__side {
    display: none;
  }

  header {
    margin-left: -80px;
  }

  .movil {
    display: block;
  }
}

@media screen and (min-width: 768px) {
  .barras {
    display: none;
  }

  .movil {
    display: none;
  }
}
</style>
