<template>
  <!-- Section Cards -->
  <section class="authentication">
    <div class="auth-body">
      <h1 class="text-uppercase fw-500 mb-4 text-center font-22">
        Reset Password
      </h1>
      <form class="auth-form" @submit.prevent="submit">
        <AlertSuccess :form="form"
          >The Password has been reset
          <nuxt-link to="/auth/login">Proccerd to login</nuxt-link>
        </AlertSuccess>
        <div class="form-group">
          <input
            type="text"
            name="email"
            disabled
            readonly
            class="form-control form-control-lg font-14 fw-300"
            placeholder="Email"
            v-model.trim="form.email"
            :class="{ 'is-invalid': form.errors.has('email') }"
          />
          <HasError :form="form" field="email" />
        </div>
        <div class="form-group">
          <input
            type="password"
            name="password"
            class="form-control form-control-lg font-14 fw-300"
            placeholder="Password"
            v-model.trim="form.password"
            :class="{ 'is-invalid': form.errors.has('password') }"
          />
          <HasError :form="form" field="password" />
        </div>
        <div class="form-group">
          <input
            type="password"
            name="password_confirmation"
            class="form-control form-control-lg font-14 fw-300"
            placeholder="Confirm New Password"
            v-model.trim="form.password_confirmation"
            :class="{ 'is-invalid': form.errors.has('password_confirmation') }"
          />
          <HasError :form="form" field="password_confirmation" />
        </div>

        <div class="text-right">
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
            Reset Password
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
      form: new Form({
        email: '',
        password: '',
        password_confirmation: '',
        token: '',
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
        .post(`/guest/password/reset`)
        .then((res) => {
          this.form.reset()
        })
        .catch((err) => console.log(err))
    },
  },
  created() {
    this.form.email = this.$route.query.email
    this.form.token = this.$route.params.token
  },
  middleware: ['guest'],
}
</script>
