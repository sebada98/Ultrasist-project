<template>
  <div>
    <SideBarKixComponent />
    <div class="container-fluid p-5 mx-3">

      
        <div class="row">
          <div class="col-sm-12 col-lg-6 m-auto">
          <div class="card">
            <h2>oficio</h2>
            <div class="card-body">
              <!-- <CarruselComponent :arregloDeImagenes="imagenes" /> -->
              <CarruselComponent2 />
            </div>
          </div>
        </div>
        
      </div>

    </div>
  </div>
</template>

<script>
import SideBarKixComponent from "@/components/SideBarKixComponent.vue";
import CarruselComponent2 from "@/components/CarruselComponent2.vue";


export default {
  components: {
    SideBarKixComponent,
    CarruselComponent2
  },
};
</script>
