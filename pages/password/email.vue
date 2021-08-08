<template>
  <!-- Section Cards -->
  <section class="authentication">
    <div class="auth-body">
      <h1 class="text-uppercase fw-500 mb-4 text-center font-22">
        Resend Password Reset
      </h1>
      <form class="auth-form" @submit.prevent="submit">
        <AlertSuccess
          :form="form"
          message="The Password Reset Email Has Been Sent"
        />
        <div class="form-group">
          <input
            type="text"
            name="email"
            class="form-control form-control-lg font-14 fw-300"
            placeholder="Email"
            v-model.trim="form.email"
            :class="{ 'is-invalid': form.errors.has('email') }"
          />
          <HasError :form="form" field="email" />
        </div>

        <div class="text-center">
          <button
            type="submit"
            class="
              btn btn-primary
              primary-bg-color
              font-16
              fw-500
              text-uppercase
            "
            :disabled="form.busy"
          >
            <span v-if="form.busy">
              <i class="fas fa-spinner fa-spin"></i>
            </span>
            Resend Password Reset Link
          </button>
        </div>
      </form>
    </div>
  </section>
  <!-- End Cards -->
</template>

<script>
import HasError from 'vform/src/components/bootstrap5/HasError.vue'
import Button from 'vform/src/components/bootstrap5/Button.vue'
import AlertErrors from 'vform/src/components/bootstrap4/AlertErrors.vue'
import AlertError from 'vform/src/components/bootstrap4/AlertError.vue'
import AlertSuccess from 'vform/src/components/bootstrap4/AlertSuccess.vue'
import Form from 'vform'
export default {
  data() {
    return {
      form: this.$vform({
        email: '',
        status: '',
      }),
    }
  },
  components: {
    HasError,
    Button,
    AlertErrors,
    AlertSuccess,
    AlertError,
  },
  methods: {
    submit() {
      this.form
        .post(`/guest/password/email`)
        .then((res) => (this.status = res.data.status) && this.form.reset())
        .catch((err) => console.log(err))
    },
  },
  middleware: ['guest'],
}
</script>
