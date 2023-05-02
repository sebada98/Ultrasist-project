<template>
  <div class="content bg-dark">
    <div class="centra">
      <h3 class="text-light">{{ titulo }}</h3>
      <div class="card bg-secondary">
        <div v-if="isMobile() || this.imgSelected == true">
          <div class="img-user" @click="fireChooser">
            <img
              class="croppedImage"
              v-if="profilePicture"
              :src="profilePicture"
              alt="Cropped Image"
            />
            <hr />
            <a class="btn btn-warning">Cambiar Imagen</a>
          </div>
        </div>
        <div v-else @click="fireChooser">
          <div
            class="card fondo-card"
            @drop.prevent="onDrop($event)"
            @dragover.prevent="dragover = true"
            @dragenter.prevent="dragover = true"
            @dragleave.prevent="dragover = false"
            :class="{ 'drag': dragover }"
          >
            <div>
              <div class="row">
                <div class="col">
                  <br />
                  <i class="fa-solid fa-cloud-arrow-up fa-3x text-light"></i>
                </div>
              </div>
              <div class="row">
                <div class="col">
                  <p class="text-light">
                    <br />Arrastre y suelte una imagen en esta área<br />O haga
                    click para seleccionar una
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div style="margin: 20px;">
          <div class="row">
            <div class="col">
              <button
                :disabled="buttonDisabled"
                @click="saveImage"
                class="btn btn-success"
                prepend-icon="mdi-cloud-upload"
              >
                Guardar
              </button>
            </div>
            <div class="col">
              <a
                class="btn btn-danger"
                :disabled="loading"
                :href="returnUrl"
              >
                Cancelar
              </a>
            </div>
          </div>
        </div>
      </div>

      <imageCropper
        :asp-rad="aspectRadio"
        ref="imageCropper"
        @img-error="imgError"
        @procesa-imagen="procesa"
        @cancela-imagen="cancelaImagen"
        @cancela-en-el-hijo="cancelaEnElHijo"
      />

      <div :class="loader" />

      <MessageComponent
        ref="message01"
        alertType="3"
        duration="4000"
        :text="errorText"
        iconType="1"
        style="max-width: 600px;"
      />
    </div>
  </div>
</template>
    
  <script>
    import axios from "axios"
    import store from "@/store"
    import router from "@/router"
    
    import def from "@/assets/default.jpg"
    import ImageCropper from "@/components/ImageCropper.vue"
    
    export default {
      name: "UploadIMG",
      props: ['radio', 'returnUrl', 'apiUrl', 'titulo'],
      data() {
        return {
          userId: store.state.userData.idUser,
          jwt: store.state.userData.jwt,
          profilePicture: def,
          aspectRadio: parseFloat(this.radio),
          loading: false,
          loader: 'ancho',
          dragover: false,
          uploadedFiles: [],
          imgSelected: false,
          cropperSelected: false,
          buttonDisabled:true,
          errorText:'',
          multiple: {
            type: Boolean,
            default: false
          } 
        };
      },
      components: {
        ImageCropper
      },
      methods: {
        isMobile() {      
          return /android|webos|iphone|ipad|ipod|blackberry|opera mini|iemobile/i.test(
            navigator.userAgent.toLowerCase()
          );
        },
        resetImage() {
          this.profilePicture = "";
          this.imageBlob = "";
          this.cropperSelected="";
        },
        saveImage: function () {
          this.loader = 'loader'
          this.loading = true;
          let formd = new FormData();
          formd.append("file", this.imageBlob, this.imageBlob.name);
          const headers = {
            "Content-Type": "multipart/form-data",
            "Access-Control-Allow-Origin": "*",
            jwt: this.jwt,
            idUser: store.state.userData.idUser,
          };
    
          axios
            .post(this.apiUrl, formd, { headers })
            .then((response) => {
              response;
              this.loading = false;
              this.loader = 'ancho'
              this.resetImage();
              router.push(this.returnUrl);
            })
            .catch((error) => {
              this.loading = false;
              this.loader = 'ancho'
              this.resetImage();
              this.errorText = error.response ? error.response.data.exceptionLongDescription : error
              console.log(error)
              this.$refs.message01.presenta()
            });
        },
        procesa(imagen, imageBlob) {
          this.profilePicture = imagen;
          this.imageBlob = imageBlob;      
          this.imgSelected = true;
          this.cropperSelected = true;
          this.buttonDisabled = false;
        },
        imgError(msg) {
          this.errorText = msg
          this.$refs.message01.presenta()
        },
        cancelaImagen() {
          if (!this.cropperSelected) {
            this.imgSelected = false;        
          }
          this.cropperSelected = false;            
        },
        cancelaEnElHijo() {
          this.buttonDisabled=true;
        },
        fireChooser() {
          this.$refs.imageCropper.fireChooser();
        },
        removeFile(fileName) {
          // Find the index of the
          const index = this.uploadedFiles.findIndex(
            (file) => file.name === fileName
          );
          // If file is in uploaded files remove it
          if (index > -1) this.uploadedFiles.splice(index, 1);
        },
        onDrop(e) {
          this.dragover = false;
          // If there are already uploaded files remove them
          if (this.uploadedFiles.length > 0) this.uploadedFiles = []
          // If user has uploaded multiple files but the component is not multiple throw error
          if (!this.multiple && e.dataTransfer.files.length > 1) {
            this.$store.dispatch("addNotification", {
              message: "Only one file can be uploaded at a time..",
              colour: "error",
            });
          }
          else {
            this.imgSelected = this.$refs.imageCropper.setImageDrag(e.dataTransfer.files.item(0))
          }
        },
      },
    };
    </script>
    
    <style scoped>
    .bg-dark {
      background-color: #343a40;
    }
  
    .content.bg-dark {
      min-height: 100vh;
      padding: 5% 0;
    }
  
    .centra {
      max-width: 600px;
      margin: auto;
      padding-top: 2%;
      padding-bottom: 200px;
    }
  
    .fondo-card {
      background-color: #343a40;
      border: 1px solid #6c757d;
      color: white;
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
    background-color: #6c757d;
    color: white;
  }
  h3.text-light {
    margin-bottom: 1rem;
    color: #f8f9fa;
  }
</style>  