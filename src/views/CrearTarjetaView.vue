<template>
  <div>
    <SideBarKixComponent />
    <div class="container-fluid p-4 mx-3">
      <div class="card  card-body col-lg-6 m-auto">
        <h2>Agrega Una Forma de Pago</h2>
        <div class="row card-body">
          <div class="col-sm-12 col-lg-12 mb-4">
            <label for="inputState" class="form-label"
              >Tipo de Tarjeta<i
                class="fa-solid fa-location-arrow mx-2"
                style="color: #1d43f2"
              ></i
            ></label>
            <select id="inputState" class="form-select">
              <option selected>Elegir colonia</option>
              <option>Tarjeta de Credito</option>
              <option>Tarjeta de debito</option>
            </select>
          </div>

          <div class="col-sm-12 col-lg-12 mb-1">
            <label for="inputEmail4" class="form-label"
              >Nombre Del Propeitario<i
                class="fa-solid fa-map-pin"
                style="color: #1d43f2"
              ></i
            ></label>
            <input type="email" class="form-control mb-1" id="inputEmail4" />
          </div>
          <div class="col-sm-12 col-lg-12 mb-1">
            <label for="inputEmail4" class="form-label"
              >Numero De Tarjeta<i
                class="fa-solid fa-map-pin"
                style="color: #1d43f2"
              ></i
            ></label>
            <input type="email" class="form-control mb-1" id="inputEmail4" />
          </div>

          <div class="col-sm-12 col-lg-4 mb-1">
            <label for="inputEmail4" class="form-label"
              >MM<i class="fa-solid fa-map-pin" style="color: #1d43f2"></i
            ></label>
            <input type="email" class="form-control mb-3" id="inputEmail4" />
          </div>

          <div class="col-sm-12 col-lg-4 mb-4">
            <label for="inputEmail4" class="form-label"
              >YY<i class="fa-solid fa-map-pin" style="color: #1d43f2"></i
            ></label>
            <input type="email" class="form-control mb-3" id="inputEmail4" />
          </div>

          <div class="col-sm-12 col-lg-4 mb-2">
            <label for="inputEmail4" class="form-label"
              >CVC<i class="fa-solid fa-map-pin" style="color: #1d43f2"></i
            ></label>
            <input type="email" class="form-control mb-3" id="inputEmail4" />
          </div>

          <button class="btn btn-primary my-3">Agregar Tarjeta</button>
          <button class="btn btn-danger my-3">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SideBarKixComponent from "@/components/SideBarKixComponent.vue";

export default {
  components: {
    SideBarKixComponent,
  },
};
</script>
