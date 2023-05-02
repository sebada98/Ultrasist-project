z<template>
  <!-- Carousel wrapper -->
  <div
    id="carouselExampleIndicators"
    class="carousel slide carousel-fade ancho "
    data-bs-ride="carousel"
  >
    <!-- Slides -->
    <div class="carousel-inner mb-5">
      <div style="height:400px;">
        <img src="https://picsum.photos/400/400" class="d-block  alto" alt="..." style="height: 100%; width: 100%;"/>
        <img src="https://picsum.photos/399/400" class="d-block  alto" alt="..." style="height: 100%; width: 100%;"/>
        <img src="https://picsum.photos/398/400" class="d-block  alto" alt="..." style="height: 100%; width: 100%;"/>
        <img src="https://picsum.photos/397/400" class="d-block  alto" alt="..." style="height: 100%; width: 100%;"/>
      </div>
    </div>
    <!-- Slides -->

    <!-- Controls -->
    <button
      class="carousel-control-prev"
      type="button"
      data-bs-target="#carouselExampleIndicators"
      data-bs-slide="prev"
    >
      <span class="carousel-control-prev-icon" aria-hidden="true"></span>
      <span class="visually-hidden">Previous</span>
    </button>
    <button
      class="carousel-control-next"
      type="button"
      data-bs-target="#carouselExampleIndicators"
      data-bs-slide="next"
    >
      <span class="carousel-control-next-icon" aria-hidden="true"></span>
      <span class="visually-hidden">Next</span>
    </button>
    <!-- Controls -->

    <!-- Thumbnails -->
    <div class="carousel-indicators overflow-scroll" style="margin-bottom:-200px">
      <div>
        <button
          type="button"
          data-bs-target="#carouselExampleIndicators"
          :data-bs-slide-to=index
          :class=selecciona2(index)
          style="width: 150px"
        >
          <img class="d-block w-100 img-fluid " src="https://picsum.photos/100/100" />
        </button>
      </div>
    </div>
  </div>
  <!-- Carousel wrapper -->
</template>

<script>
import axios from "axios";
const inicio = "http://localhost/";

export default {
  props: ["arregloDeImagenes"],

  data: function () {
    return {
    
      carruselActivo: "carousel-item active",
      carruselInactivo: "carousel-item",
    };
  },
  mounted() {
    this.carga();
  },
  methods: {
    selecciona: function (index) {
      return (index == 0)? this.carruselActivo : this.carruselInactivo;
    },
    carga: function () {
      //var id = this.$route.query.id
      var id = this.$route.params.id
      //var id = 1;
      axios
        .get("http://localhost:8080/kix/product?id=" + id, {})
        .then((response) => {
          console.log(response.data);
          this.principal = response.data.producto;
          this.caracteristicas = response.data.caracteristicas;
          this.imagenes = response.data.imagenes;
        });
    },

    selecciona2: function (index) {
      return (index == 0)? "active" : "inactive";
    },
    une: function (img) {
      return inicio + img;
    },
  },
};
</script>
