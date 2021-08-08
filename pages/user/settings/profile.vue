<template>
  <div class="setting-block">
    <div class="setting-title font-16 fw-500">Profile</div>
    <div class="setting-body white-bg-color">
      <div class="col-md-6">
        <form class="custom-form" @submit.prevent="update">
          <AlertSuccess :form="form" message="Your changes have been saved!" />
          <div class="form-group">
            <label class="font-14 fw-500">Name</label>
            <input
              type="text"
              name="name"
              class="form-control form-control-lg font-14 fw-300"
              placeholder="Full Name"
              :class="{ 'is-invalid': form.errors.has('name') }"
              v-model.trim="form.name"
            />
            <HasError :form="form" field="name" />
          </div>
          <div class="form-group">
            <label class="font-14 fw-500">Tag Line</label>
            <input
              type="text"
              class="form-control form-control-lg font-14 fw-300"
              placeholder="eg. Senior Graphics Designer"
              name="tagline"
              :class="{ 'is-invalid': form.errors.has('tagline') }"
              v-model.trim="form.tagline"
            />
            <HasError :form="form" field="tagline" />
          </div>

          <div class="form-group">
            <label class="font-14 fw-500">About me</label>
            <textarea
              class="form-control font-14 fw-300"
              rows="5"
              placeholder="Tell us about you"
              name="about"
              :class="{ 'is-invalid': form.errors.has('about') }"
              v-model.trim="form.about"
            ></textarea>
            <HasError :form="form" field="about" />
          </div>

          <div class="form-group">
            <label class="font-14 fw-500">Location</label>
            <input
              type="text"
              name="formatted_address"
              class="form-control form-control-lg font-14 fw-300"
              placeholder="eg New York, USA"
              :class="{ 'is-invalid': form.errors.has('formatted_address') }"
              v-model.trim="form.formatted_address"
            />
            <HasError :form="form" field="formatted_address" />
          </div>

          <div class="custom-control custom-checkbox mt-2 font-14 fw-300">
            <input
              type="checkbox"
              class="custom-control-input"
              id="checkBox1"
              :class="{ 'is-invalid': form.errors.has('available_to_hire') }"
              v-model.trim="form.available_to_hire"
            />
            <label class="custom-control-label" for="checkBox1"
              >Available to hire?</label
            >
            <HasError :form="form" field="available_to_hire" />
          </div>
          <div class="mt-2 font-14 fw-300 text-right">
            <base-button class="btn btn-primary" :loading="form.busy"
              >Save</base-button
            >
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import AlertSuccess from 'vform/src/components/bootstrap4/AlertSuccess.vue'
import HasError from 'vform/src/components/bootstrap5/HasError.vue'
import Form from 'vform'

export default {
  components: {
    AlertSuccess,
    HasError,
  },
  data() {
    return {
      form: new Form({
        name: '',
        about: '',
        tagline: '',
        formatted_address: '',
        available_to_hire: false,
      }),
    }
  },

  methods: {
    update() {
      this.form
        .put('/auth/Setting/profile')
        .then((res) => console.log(res))
        .catch((err) => console.log(err))
    },
  },
  mounted() {
    Object.keys(this.form).forEach((k) => {
      if (this.$auth.user.hasOwnProperty(k)) {
        this.form[k] = this.$auth.user[k]
      }
    })
  },
}
</script>
