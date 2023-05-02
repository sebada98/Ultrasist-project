<template>
  <div>
    <SideBarKixComponent/>
    <div class="container-fluid p-4 mx-3">
      <div class="card card-body bg-dark text-light">
        <h2><i class="fa-solid fa-rectangle-ad" style="color: #1d4cf2;"></i> Crea tu Anuncio!</h2>
        <form action="">
          <div class="row">
            <div class="col-md-6 mb-4">
              <label for="formGroupExampleInput" class="form-label">Descripcion pequeña:</label>
              <input
                type="text"
                class="form-control bg-dark text-light"
                id="formGroupExampleInput"
                placeholder="Hacemos los mejores trabajos y cobramos barato"
              />
              <label for="floatingTextarea2" class="form-label mt-4">Descripcion completa:</label>
              <textarea
                class="form-control bg-dark text-light"
                id="floatingTextarea2"
                style="height: 100px"
                placeholder="Escriba aquí la descripción completa"
              ></textarea>
            </div>
            <div class="col-md-6">
              <div class="row">
                <div class="col-sm-6 mb-4">
                  <label for="inputEmail4" class="form-label">Codigo Postal <i class="fa-solid fa-map-pin" style="color: #1d43f2;"></i></label>
                  <input type="email" class="form-control bg-dark text-light" id="inputEmail4" />
                </div>
                <div class="col-sm-6 mb-4">
                  <label for="inputText" class="form-label">Oficio <i class="fa-solid fa-hammer" style="color: #1d43f2;"></i></label>
                  <input type="text" class="form-control bg-dark text-light" id="inputPassword4" />
                </div>
              </div>
              <div class="row mt-4 mb-4 justify-content-around">
                <div class="col-sm-6 col-md-5 mb-4">
                  <a href="/ui/upload/" class="btn btn-primary w-100">Insertar Imagen <i class="fa-solid fa-file-image" style="color: #ffffff;"></i></a>
                </div>
                <div class="col-sm-6 col-md-5">
                  <button class="btn btn-primary w-100">Insertar Video <i class="fa-regular fa-file-video" style="color: #ffffff;"></i></button>
                </div>
              </div>
            </div>
          </div>
          <h2 class="my-4">Imagenes</h2>
          <div class="d-flex gap-3 flex-wrap justify-content-around divi">
            <div v-for="imagen in imagenes" v-bind:key="imagen.id" id="contenedor">
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
          <h2 class="my-4">Videos</h2>
          <div class="d-flex flex-wrap">
            <img src="https://picsum.photos/125/125" alt="" class="m-1" />
            <img src="https://picsum.photos/125/125" alt="" class="m-1" />
            <img src="https://picsum.photos/125/125" alt="" class="m-1" />
            <img src="https://picsum.photos/125/125" alt="" class="m-1" />
            <img src="https://picsum.photos/125/125" alt="" class="m-1" />
            <img src="https://picsum.photos/125/125" alt="" class="m-1" />
          </div>
          <h4 class="my-5">Datos del negocio <i class="fa-solid fa-house" style="color: #1d43f2;"></i></h4>
          <div class="row">
            <div class="col-sm-12 col-lg-4">
              <label for="inputEmail4" class="form-label">Telefono del Negocio <i class="fa-solid fa-map-pin" style="color: #1d43f2;"></i></label>
              <input type="email" class="form-control bg-dark text-light mb-3" id="inputEmail4" />
            </div>
            <div class="col-sm-12 col-lg-4">
              <label for="inputCity" class="form-label">Correo Electronico del negocio</label>
              <input type="text" class="form-control bg-dark text-light" id="inputCity" placeholder="mi_negocio@example.com" />
            </div>
            <div class="col-sm-12 col-lg-4 mb-4">
              <label for="inputCity" class="form-label">Nickname o apodo</label>
              <input type="text" class="form-control bg-dark text-light" id="inputCity" placeholder="Don chuy" />
            </div>
          </div>
          <div class="row d-flex justify-content-around mt-4 mb-4">
            <div class="col-sm-12 col-lg-5 mb-4">
              <a href="http://localhost:8080/ui/ine" class="btn btn-primary w-100">
                <span><i class="fa-solid fa-address-card mx-2" style="color: #ffffff;"></i></span> Credencial INE
              </a>
            </div>
            <div class="col-sm-12 col-lg-5 mb-4">
              <a href="http://localhost:8080/ui/pago" class="btn btn-primary w-100">
                <span><i class="fa-solid fa-credit-card mx-2" style="color: #ffffff;"></i></span> Agregar forma de pago
              </a>
            </div>
          </div>
          <div class="col-sm-12">
            <button type="submit" class="btn btn-primary my-4">
              Enviar Datos y Crear Anuncio
            </button>
          </div>
        </form>
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
    SideBarKixComponent,
  },
  data() {
    return {
      imagenes: [],
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
        .get(
          "https://upload.qbits.mx/api/up/get-user-all-images/" +
            this.idUser
        )
        .then((response) => {
          this.imagenes = response.data;
          this.loadedImages = this.imagenes.length;
          console.log(this.imagenes);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    elimina: function (imagen) {
      this.loader = "loader";
      let tarjeta = document.querySelectorAll(".elemento");
      console.log("La imagen se ha eliminado correctamente");
      // Eliminar la imagen de la matriz de imágenes
      const index = this.imagenes.indexOf(imagen);
      //this.imagenes.splice(index,1);
      console.log(index);
      tarjeta[index].textContent = "";
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
          location.reload();
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
};
</script>

<style>
body {
  background-color: #212529;
}

img {
  width: 125px;
  height: 125px;
}

.form-control.bg-dark.text-light,
.form-control.bg-dark.text-light::placeholder {
  background-color: #343a40;
  color: #ccc;
}

.form-control.bg-dark.text-light:focus {
  background-color: #343a40;
  color: #ccc;
  box-shadow: none;
}

.card {
  background-color: #343a40;
  border-color: #454d55;
}

.card-body {
  color: #ccc;
}
</style>
