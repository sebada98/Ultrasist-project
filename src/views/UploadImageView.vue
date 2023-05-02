<template>
  <div class="content">
    <UploadImageComponent
    class ="centra" 
    titulo="subir imagen para anuncios" 
    radio="3/4" 
    apiUrl="https://upload.qbits.mx/api/up/upload-image"
    returnUrl="/ui/anuncio"/>
    <FooterComponent class="down" />
  </div>
</template>

<script>
import UploadImageComponent from '@/components/UploadImageComponent.vue';

export default {
  components: {
    UploadImageComponent
  }
}
</script>

<style scoped>
.centra {
  max-width: 600px;
  margin: auto;
  padding-top: 10%;
  padding-bottom: 200px;
}
.fondo-card {
  background-color: #efefff;
}
.croppedImage {
  display: block;
  width: 80%;
  height: auto;
}
.img-user {
  text-align: center;
  padding: 20px;
}
img {
  height: 300px;
  margin: 0 auto;
}
.drag {
  background-color: gray;
  color: white;
}
</style>
