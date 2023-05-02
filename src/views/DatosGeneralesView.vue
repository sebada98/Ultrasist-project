<template>
  <div>
    <SideBarKixComponent/>
    <div class="container-fluid p-4 mx-3">
      <div class="card bg-dark text-light">
        <h2 class="p-3">
          Datos Personales<i
            class="fa-solid fa-circle-question fs-3 mx-1"
            style="color: #90caf9"
          ></i>
        </h2>
        <div v-if="loadedImages < maxImagesAllowed" class="text-center">
          <a href="/ui/upload/principal" class="btn btn-outline-light p-1 mb-5">
            <div>
              <img src="../assets/user.png" alt="" />
              <h6 class="text-warning">Selecciona una imagen de perfil</h6>
            </div>
          </a>
        </div>

        <div v-else>
          <div class="d-flex gap-3 flex-wrap m-5 justify-content-around divi">
            <div
              id="contenedor"
            >
              <div class="elemento position-relative">
                <img
                  :src="une(imagen.fullHttpUploadUrl)"
                  alt=""
                  class="rounded img"
                />
                <i
                  class="fa-solid fa-circle-xmark fa-bounce fa-2xl position-absolute top-0 end-0 border border-light"
                  style="color: #ec091f"
                  @click="elimina(imagen)"
                ></i>
              </div>
            </div>
          </div>
        </div>

        <div class="card-body">
          <form class="row g-3">
            <div class="col-sm-12 col-lg-4">
              <label for="inputEmail4" class="form-label"
                >Nombre<i class="fa-solid fa-pen mx-2" style="color: #90caf9"></i
              ></label>
              <input type="text" class="form-control" id="inputEmail4" />
            </div>
            <div class="col-sm-12 col-lg-4">
              <label for="inputPassword4" class="form-label"
                >Apellido Paterno<i
                  class="fa-solid fa-pen mx-2"
                  style="color: #90caf9"
                ></i
              ></label>
              <input type="text" class="form-control" id="inputPassword4" />
            </div>
            <div class="col-sm-12 col-lg-4">
              <label for="inputPassword4" class="form-label"
                >Apellido Materno<i
                  class="fa-solid fa-pen mx-2"
                  style="color: #90caf9"
                ></i
              ></label>
              <input type="text" class="form-control" id="inputPassword4" />
            </div>
            <div class="col-sm-12 col-lg-6 mt-4">
              <label for="inputPassword4" class="form-label"
                >Telefono Personal<i
                  class="fa-solid fa-phone mx-2"
                  style="color: #90caf9"
                ></i
              ></label>
              <input type="text" class="form-control" id="inputPassword4" />
            </div>
            <div class="col-sm-12 col-lg-6 mt-4">
              <label for="inputPassword4" class="form-label"
                >Correo Personal<i
                  class="fa-regular fa-at mx-2"
                  style="color: #90caf9"
                ></i
              ></label>
              <input type="text" class="form-control" id="inputPassword4" />
            </div>

            <div class="col-sm-12 col-lg-4">
              <label for="inputAddress2" class="form-label"
                >Fecha de nacimiento<i
                  class="fa-solid fa-calendar-days mx-2"
                  style="color: #90caf9"
                ></i
              ></label>
              <input
                type="text"
                class="form-control"
                id="inputAddress2"
                placeholder="12 enero 1987"
              />
            </div>
            <div class="col-sm-12 col-lg-4">
              <label for="inputCity" class="form-label"
                >Estado<i
                  class="fa-solid fa-map-pin mx-2"
                  style="color: #90caf9"
                ></i
              ></label>
              <input type="text" class="form-control" id="inputCity" />
            </div>
            <div class="col-sm-12 col-lg-4">
              <label for="inputCity" class="form-label"
                >Municipio<i
                  class="fa-solid fa-map-location-dot mx-2"
                  style="color: #90caf9"
                ></i
              ></label>
              <input type="text" class="form-control" id="inputCity" />
            </div>
            <div class="col-sm-12 col-lg-6">
              <label for="inputState" class="form-label"
                >Colonia<i
                  class="fa-solid fa-location-arrow mx-2"
                  style="color: #90caf9"
                ></i
              ></label>
              <select id="inputState" class="form-select">
                <option selected>Elegir colonia</option>
                <option>...</option>
              </select>
            </div>
            <div class="col-sm-12 col-lg-6">
              <label for="inputZip" class="form-label"
                >Codigo Postal<i
                  class="fa-solid fa-envelopes-bulk mx-2"
                  style="color: #90caf9"
                ></i
              ></label>
              <input type="text" class="form-control" id="inputZip" />
            </div>

            <div class="col-12">
              <a type="submit" class="btn btn-outline-light mt-4 mb-4"
                >Cambiar contraseña y Usuario
                <i class="fa-solid fa-lock mx-2" style="color: #ffffff"></i
              ></a>
            </div>
            <div class="col-12">
              <button type="submit" class="btn btn-primary">Enviar</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SideBarKixComponent from '@/components/SideBarKixComponent.vue';
import axios from "axios";
import store from "@/store";

export default {
  components: {
    SideBarKixComponent
  },
  data() {
    return {
      imagen:{},
      idUser: store.state.userData.idUser,
      maxImagesAllowed: 1,
      loadedImages: 0,
    };
  },
  mounted() {
    this.carga();
  },
  methods: {
    carga() {
      axios
      //   .get(
      //     "https://upload.qbits.mx/api/up/get-user-pricipal-image/" +
      //       this.idUser
      // )
        .get(
          "https://upload.qbits.mx/api/up/get-user-pricipal-image/49"
        )
        .then((response) => {
          this.imagen = response.data;
          this.loadedImages = this.imagen.length;
          console.log(this.imagen);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    elimina: function (imagen) {
      this.loader = "loader";
      let tarjeta = document.querySelector(".elemento");
      console.log("La imagen se ha eliminado correctamente");
      // Eliminar la imagen de la matriz de imágenes
      //this.imagenes.splice(index,1);
      tarjeta.textContent = "";
      console.log(imagen.id);
      console.log("estoy llegando");
      const options = {
        method: "DELETE",
        url: `https://upload.qbits.mx/api/up/delete-media/${imagen.id}`,
        headers: {
          idMedia: imagen.id,
          IdUser: store.state.userData.idUser,
          jwt: store.state.userData.jwt,
        },
      };
      console.log("estoy llegando");
      axios
        .request(options)
        .then(function (response) {
          this.loader = "none";
          console.log(response.data);
          this.delete = response.data;
        })
        .catch(function (error) {
          this.loader = "none";
          console.error(error);
        });
    },
    une(Nombreimg) {
      return `https://media.visitanos.net/image${Nombreimg}`;
    },
  },
}
</script>