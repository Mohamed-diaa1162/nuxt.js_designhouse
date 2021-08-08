<template>
  <div>
    <!-- Start Hero -->
    <section class="hero text-center bg-secondary text-white">
      <div class="container">
        <h1 class="font-28 fw-600 text-uppercase">Upload a design</h1>
      </div>
    </section>
    <!-- End Hero -->

    <!-- Upload Shot -->
    <div class="upload-shot">
      <div class="container">
        <div class="row justify-content-center align-items-center text-center">
          <div class="col-md-6">
            <div class="card bg-white shadow-sm">
              <div class="d-flex flex-column justify-content-center p-1">
                <div class="alert alert-danger" v-if="error">
                  <p>An error ocuurred during the upload process</p>
                  <p>{{ error }}</p>
                </div>
                <slim-cropper :options="slimOptions">
                  <input type="file" name="image" />
                </slim-cropper>
                <div class="mt-2 text-success caption-sm" v-if="uploading">
                  <i class="fas fa-spinner fa-spin"></i>
                </div>
              </div>
            </div>
          </div>
          <div class="upload-para mt-2">
            <p class="font-14 fw-400">
              JPG, GIF or PNG. Dribbble shots are
              <strong>400 × 300</strong> pixels or
              <strong>800 × 600</strong> (for HiDPI displays). Also the image
              size should be a maximum of 2MB
            </p>
            <p class="font-12 fw-300">
              If your image is bigger than the sizes above, we’ll help you crop
              it.
            </p>
          </div>
        </div>
      </div>
    </div>

    <!-- End Upload Shot -->
  </div>
</template>

<script>
import Slim from '@/components/slim/slim.vue'
export default {
  components: {
    'slim-cropper': Slim,
  },
  middleware: 'auth',
  data() {
    return {
      slimOptions: {
        service: this.slimService,
        post: 'output',
        defaultInputName: 'image',
        minSize: '800,600',
        label: 'Select Image ...',
        maxFileSize: 2, // Value is 2MB
      },
      uploading: false,
      error: '',
    }
  },

  methods: {
    slimService(formdata, progress, success, failure, slim) {
      this.uploading = true
      this.$axios
        .post('/auth/upload', formdata)
        .then((res) => {
          console.log(res.data.id)
          this.$router.push({
            path: `/designs/update/${res.data.id}`,
          })
        })
        .catch((err) => {
          console.log(err)
          const message = err.response.data.errors
          this.error = message[Object.keys(message)[0]][0]
        })
        .finally(() => (this.uploading = false))
    },
  },
}
</script>
