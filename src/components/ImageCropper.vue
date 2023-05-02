<template>
  <!-- Modal for us to crop the given image -->
  <!-- TODO Make this componente reusable -->
  <div class="modalx-overlay" v-show="muestra">
    <!-- Hidden input file chooser accepting only images. For example: jpg, png, etc-->
    <input
      style="display: contents; background-color: yellow"
      type="file"
      @change="setImage"
      ref="fileInput"
      accept="image/*"
    />

    <div class="modalx">
      <div class="modal-headerX">
        <div class="row">
          <div class="col">
            <a href="#" class="modal-header-close" @click="toogleVentana">X</a>
          </div>
        </div>
      </div>
      <div class="img-cropper">
        <!-- Cropper -->
        <vue-cropper
          ref="cropper"
          :src="selectedFile"
          :img-style="{ width: 'auto', height: '266' }"
          :aspectRatio="aspRad"
        >
        </vue-cropper>
        <!-- Toolbox -->
        <div class="container bg-dark2 text-light toolSet">
          <div class="tools">
            <div class="d-flex">
              <div class="icons">
                <a
                  href="#"
                  @click.prevent="zoom(0.3)"
                  title="zoom in"
                  class="link"
                >
                  <i class="fa fa-search-plus" aria-hidden="true"></i>
                </a>
              </div>
              <div class="icons">
                <a
                  href="#"
                  @click.prevent="zoom(-0.3)"
                  title="zoom out"
                  class="link"
                >
                  <i class="fa fa-search-minus" aria-hidden="true"></i>
                </a>
              </div>
              <div class="icons">
                <a
                  href="#"
                  @click.prevent="rotate(45)"
                  title="rotate right"
                  class="link"
                >
                  <i class="fa fa-redo" aria-hidden="true"></i>
                </a>
              </div>
              <div class="icons">
                <a
                  href="#"
                  @click.prevent="rotate(-45)"
                  title="rotate left"
                  class="link"
                >
                  <i class="fa fa-undo" aria-hidden="true"></i>
                </a>
              </div>
              <div class="icons">
                <a
                  href="#"
                  @click.prevent="move(-10, 0)"
                  title="move left"
                  class="link"
                >
                  <i class="fa fa-arrow-left" aria-hidden="true"></i>
                </a>
              </div>
              <div class="icons">
                <a
                  href="#"
                  @click.prevent="move(10, 0)"
                  title="move right"
                  class="link"
                >
                  <i class="fa fa-arrow-right" aria-hidden="true"></i>
                </a>
              </div>
              <div class="icons">
                <a
                  href="#"
                  @click.prevent="move(0, -10)"
                  title="move up"
                  class="link"
                >
                  <i class="fa fa-arrow-up" aria-hidden="true"></i>
                </a>
              </div>
              <div class="icons">
                <a
                  href="#"
                  @click.prevent="move(0, 10)"
                  title="move down"
                  class="link"
                >
                  <i class="fa fa-arrow-down" aria-hidden="true"></i>
                </a>
              </div>
              <div class="icons">
                <a
                  ref="flipX"
                  href="#"
                  @click.prevent="flipX"
                  title="flip vertical"
                  class="link"
                >
                  <i class="fas fa-ruler-vertical"></i>
                </a>
              </div>
              <div class="icons">
                <a
                  ref="flipY"
                  href="#"
                  @click.prevent="flipY"
                  title="flip horizontal"
                  class="link"
                >
                  <i class="fas fa-ruler-horizontal"></i>
                </a>
              </div>
            </div>
          </div>
        </div>
        <!-- Messages -->
        <div class="container justify-content-around p-2 bg-dark3">
          <div class="row">
            <div class="col">
              <small><i class="fas fa-file"></i> {{ fileName }}</small>
            </div>
          </div>
          <div class="row">
            <div class="col">
              <small
                ><i class="fas fa-ruler"></i>
                {{ Math.trunc(fileSize / 1024) }} kilo bytes</small
              >
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <button
            type="button"
            class="button-selec"
            role="button"
            @click="cropImage"
          >
            Select
          </button>

          <button
            type="button"
            class="button-cancel"
            role="button"
            @click="cierraVentana"
          >
            Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import def from "@/assets/default.jpg";
import VueCropper from "vue-cropperjs";
import "cropperjs/dist/cropper.css";

export default {
  props: ["aspRad"],
  components: {
    VueCropper,
  },
  data() {
    return {
      profilePicture: def,
      imageBlob: def,
      selectedFile: def,
      fileName: "",
      fileNameBlob: def,
      fileSize: 0,
      ancho: 366,
      alto: 266,
      muestra: false,
      isImage: false,
    };
  },
  methods: {
    invalid(file) {
      let kb = 1024;
      this.fileName = file.name;
      this.fileSize = file.size;
      if (file.size < 16 || file.size > 9000 * kb) {
        const msg =
          "Sorry, image too small or image too large. Size detected: " +
          file.size +
          " bytes";
        this.$emit("img-error", msg);
        return true;
      }
      //if(file.height<16 || file.height>8000*kb) return "Image Height to big or image too small: " + file.height
      //if(file.width<16 || file.width>8000*kb) return "Image Width to big or image too small: " + file.width
      this.detectMimeType(file);
      if (!this.isImage) {
        const msg = "Sorry, this is not an image file :(";
        this.$emit("img-error", msg);
        return true;
      }
      return false;
    },

    /**
     * Load the mime type based on the signature of the first bytes of the file
     * @param  {File}   file        A instance of File
     * @param  {Function} callback  Callback with the result
     * @author Victor www.vitim.us
     * @date   2017-03-23
     */
    loadMime(file, callback) {
      //List of known mimes
      var mimes = [
        {
          mime: "image/jpeg",
          pattern: [0xff, 0xd8, 0xff],
          mask: [0xff, 0xff, 0xff],
        },
        {
          mime: "image/png",
          pattern: [0x89, 0x50, 0x4e, 0x47],
          mask: [0xff, 0xff, 0xff, 0xff],
        },
        // you can expand this list @see https://mimesniff.spec.whatwg.org/#matching-an-image-type-pattern
      ];

      function check(bytes, mime) {
        for (var i = 0, l = mime.mask.length; i < l; ++i) {
          if ((bytes[i] & mime.mask[i]) - mime.pattern[i] !== 0) {
            return false;
          }
        }
        return true;
      }

      var blob = file.slice(0, 4); //read the first 4 bytes of the file

      var reader = new FileReader();
      reader.onloadend = function (e) {
        if (e.target.readyState === FileReader.DONE) {
          var bytes = new Uint8Array(e.target.result);

          for (var i = 0, l = mimes.length; i < l; ++i) {
            if (check(bytes, mimes[i]))
              return callback(
                "Mime: " + mimes[i].mime + " <br> Browser:" + file.type
              );
          }

          return callback("Mime: unknown <br> Browser:" + file.type);
        }
      };
      reader.readAsArrayBuffer(blob);
    },

    detectMimeType(imageFile) {
      console.log(imageFile.name);
      var b =
        imageFile.name.includes(".png") ||
        imageFile.name.includes(".jpg") ||
        imageFile.name.includes(".jpeg") ||
        imageFile.name.includes(".webp");
      if (imageFile.name.length > 0) {
        this.isImage = b;
        return;
      }

      // The previous code is because the following didn't work:
      const reader = new FileReader();
      reader.readAsDataURL(imageFile);
      reader.onload = (eee) => {
        this.isImage = eee.explicitOriginalTarget.result.includes("image");
      };
      reader.onloadend = (eee) => {
        this.isImage = eee.explicitOriginalTarget.result.includes("image");
        /** /
          var file = tempo
            var arr = (new Uint8Array(file)).subarray(0, 4);
            var header = "";
            for(var i = 0; i < arr.length; i++) {
              header += arr[i].toString(16);
            }
            console.log(header)
            return this.getMimeType(header);
          };
          /**/
        //return tempo
      };
      //return tempo

      //  function convertImageToBase64Async(imagUrl) {
      //     return new Promise(resovle => convertImageToBase64(imgUrl, resolve))
      //  }
      // https://levelup.gitconnected.com/how-to-convert-image-to-base64-by-javascript-d110556de37f

      /*
  
  function convertImageToBase64(imgUrl, callback) {
    const image = new Image();
    image.crossOrigin='anonymous';
    image.onload = () => {
      const canvas = document.createElement('canvas');
      const ctx = canvas.getContext('2d');
      canvas.height = image.naturalHeight;
      canvas.width = image.naturalWidth;
      ctx.drawImage(image, 0, 0);
      const dataUrl = canvas.toDataURL();
      callback && callback(dataUrl)
    }
    image.src = imgUrl;
  }
  
  */
    },
    setImage(e) {
      const file = e.target.files[0];
      if (this.invalid(file)) {
        return;
      }
      this.fileNameBlob = file.name;
      if (typeof FileReader === "function") {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = (e) => {
          this.$refs.cropper.replace(e.target.result);
        };
        reader.onloadend = (f) => {
          this.toogleVentana();
          this.selectedFile = f.target.result;
        };
      } else {
        var msg = "Sorry, FileReader API not supported";
        this.$emit("img-error", msg);
      }
    },
    setImageDrag(ggg) {
      if (this.invalid(ggg)) {
        return false;
      }
      /**/
      this.fileNameBlob = ggg.name;
      if (typeof FileReader === "function") {
        const reader = new FileReader();
        reader.readAsDataURL(ggg);
        reader.onload = (eee) => {
          this.$refs.cropper.replace(eee.target.result);
        };
        reader.onloadend = (fff) => {
          this.toogleVentana();
          this.selectedFile = fff.target.result;
        };
      } else {
        var msg = "Sorry, FileReader API not supported";
        this.$emit("img-error", msg);
      }
      return true;
      /**/
    },
    getMimeType(header) {
      switch (header) {
        case "89504e47":
          return "image/png";
        case "47494638":
          return "image/gif";
        case "ffd8ffe0":
        case "ffd8ffe1":
        case "ffd8ffe2":
        case "ffd8ffe3":
        case "ffd8ffe8":
          return "image/jpeg";
        default:
          return "unknown";
      }
    },
    fireChooser() {
      this.$refs.fileInput.value = "";
      this.$refs.fileInput.click();
    },
    flipX() {
      const dom = this.$refs.flipX;
      let scale = dom.getAttribute("data-scale");
      scale = scale ? -scale : -1;
      this.$refs.cropper.scaleX(scale);
      dom.setAttribute("data-scale", scale);
    },
    flipY() {
      const dom = this.$refs.flipY;
      let scale = dom.getAttribute("data-scale");
      scale = scale ? -scale : -1;
      this.$refs.cropper.scaleY(scale);
      dom.setAttribute("data-scale", scale);
    },
    rotate(deg) {
      this.$refs.cropper.rotate(deg);
    },
    move(offsetX, offsetY) {
      this.$refs.cropper.move(offsetX, offsetY);
    },
    zoom(percent) {
      this.$refs.cropper.relativeZoom(percent);
    },
    cropImage() {
      this.profilePicture = this.$refs.cropper
        .getCroppedCanvas()
        .toDataURL("image/jpeg");
      this.$refs.cropper.getCroppedCanvas().toBlob((b) => {
        this.imageBlob = b;
        this.imageBlob.name = this.fileNameBlob;
        this.procesaImagen(this.profilePicture, this.imageBlob);
        this.toogleVentana();
      });
    },
    procesaImagen(img, imgBlob) {
      // enviando la imagen al padre:
      this.$emit("procesa-imagen", img, imgBlob);

      // pero aqui también se podría simplemente invocar al axios de guardado
      // algo asi como lo siguiente:
      //      let result = uploadComponent.uploadImage('/api/upload', img); // internamente, es un axcios call
      //      if (result.length>0) message(result)
      //console.log(test.pba())
    }, // para este método, podría haber una propiedad que indique si hacer lo primero, lo segundo o ambos
    toogleVentana() {
      // cancelar el procesamiento de la imagen cada vez que la ventana cambie: x = !x
      this.muestra = !this.muestra;
      this.cancelaImagen();
    },
    cancelaImagen() {
      this.$emit("cancela-imagen");
    },
    cierraVentana() {
      // cuando la ventana se cierre avisar via el método "cancela-en-el-hijo"
      this.muestra = false;
      this.$emit("cancela-en-el-hijo");
    },
  },
};
</script>

<style scaoped>
.close {
  text-align: right !important;
  margin-right: 10px;
}
.izquierda {
  text-align: left !important;
  margin-left: 10px;
}
.croppedImage {
  display: block;
  width: 200px;
  height: auto;
}
#black-label {
  color: #fff;
  font-size: 0.7em;
}
.close {
  margin: 10% 0 0 16px;
  cursor: pointer;
}
.link {
  color: green;
}
.link:hover {
  color: red;
}
.bg-dark2 {
  background-color: #ccc;
}
.bg-dark3 {
  background-color: #fff;
}
.foto {
  position: relative;
  background-color: #ff0000;
  display: inline-block;
  text-align: center;
  border-radius: 50%;
  overflow: hidden;
  transition: transform 0.2s;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 13px 27px -5px,
    rgba(0, 0, 0, 0.3) 0px 8px 16px -8px;
}
.foto:hover {
  transform: scale(1.1);
}
.modalx-overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  background-color: #000000da;
}
.modalx {
  background-color: white;
  height: 500px;
  width: 500px;
  margin-top: 10%;
  padding: 10px 0;
  border-radius: 6px;
}
.buttonX {
  background-color: #0a0a23;
  color: #fff;
  border: none;
  border-radius: 10px;
  padding: 3px;
  min-height: 30px;
  min-width: 120px;
  cursor: pointer;
}
.buttonX:hover {
  background-color: #002ead;
  transition: 0.7s;
}

.modal-header-close {
  text-decoration: none;
  font-weight: bold;
  color: #ff0000;
  padding-top: 45px;
}
.modal-header-close:hover {
  text-decoration: none;
  color: #ff0000;
}
.modal-headerX {
  text-align: right;
  height: 40px;
}
.icons {
  margin: 8px;
}

.button-selec {
  appearance: none;
  background-color: #2ea44f;
  border: 1px solid rgba(27, 31, 35, 0.15);
  border-radius: 6px;
  box-shadow: rgba(27, 31, 35, 0.1) 0 1px 0;
  box-sizing: border-box;
  color: #fff;
  cursor: pointer;
  display: inline-block;
  font-family: -apple-system, system-ui, "Segoe UI", Helvetica, Arial,
    sans-serif, "Apple Color Emoji", "Segoe UI Emoji";
  font-size: 14px;
  font-weight: 600;
  line-height: 20px;
  padding: 6px 16px;
  position: relative;
  text-align: center;
  text-decoration: none;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  vertical-align: middle;
  white-space: nowrap;
  margin: 10px;
}

.button-selec:focus:not(:focus-visible):not(.focus-visible) {
  box-shadow: none;
  outline: none;
}

.button-selec:hover {
  background-color: #2c974b;
}

.button-selec:focus {
  box-shadow: rgba(46, 164, 79, 0.4) 0 0 0 3px;
  outline: none;
}

.button-selec:disabled {
  background-color: #94d3a2;
  border-color: rgba(27, 31, 35, 0.1);
  color: rgba(255, 255, 255, 0.8);
  cursor: default;
}

.button-selec:active {
  background-color: #298e46;
  box-shadow: rgba(20, 70, 32, 0.2) 0 1px 0 inset;
}

.space {
  padding: 8px;
}

.button-cancel {
  appearance: none;
  background-color: #dc3545;
  border: 1px solid rgba(27, 31, 35, 0.15);
  border-radius: 6px;
  box-shadow: rgba(27, 31, 35, 0.1) 0 1px 0;
  box-sizing: border-box;
  color: #fff;
  cursor: pointer;
  display: inline-block;
  font-family: -apple-system, system-ui, "Segoe UI", Helvetica, Arial,
    sans-serif, "Apple Color Emoji", "Segoe UI Emoji";
  font-size: 14px;
  font-weight: 600;
  line-height: 20px;
  padding: 6px 16px;
  position: relative;
  text-align: center;
  text-decoration: none;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  vertical-align: middle;
  white-space: nowrap;
}

.button-cancel:focus:not(:focus-visible):not(.focus-visible) {
  box-shadow: none;
  outline: none;
}

.button-cancel:hover {
  background-color: #dc3545;
}

.button-cancel:focus {
  box-shadow: rgba(46, 164, 79, 0.4) 0 0 0 3px;
  outline: none;
}

.button-cancel:disabled {
  background-color: #dc3545;
  border-color: rgba(27, 31, 35, 0.1);
  color: rgba(255, 255, 255, 0.8);
  cursor: default;
}

.button-cancel:active {
  background-color: #dc3545;
  box-shadow: rgba(20, 70, 32, 0.2) 0 1px 0 inset;
}

.tools {
  display: flex;
  justify-content: center;
}
</style>
