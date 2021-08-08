<template>
  <!-- Section Cards -->
  <section class="authentication">
    <div class="auth-body">
      <h1 class="text-uppercase fw-500 mb-4 text-center font-22">Login</h1>
      <form class="auth-form" @submit.prevent="submit">
        <AlertError :form="form" v-if="form.errors.has('Verify')">
          {{ form.errors.get('Verify') }}
          <nuxt-link to="/verification/resend">
            Resend Verification Email
          </nuxt-link>
        </AlertError>
        <div class="form-group">
          <base-input
            v-model.trim="form.email"
            placeholder="Email"
            inputType="email"
            :form="form"
            field="email"
          />
        </div>
        <div class="form-group">
          <base-input
            v-model.trim="form.password"
            placeholder="Password"
            inputType="password"
            :form="form"
            field="password"
          />
        </div>
        <div class="mt-4 mb-4 clearfix">
          <nuxt-link
            to="/password/email"
            class="forgot-pass color-blue font-14 fw-400"
          >
            Forgot password?
          </nuxt-link>
        </div>
        <div class="text-right">
          <base-button :loading="this.form.busy">Log In</base-button>
        </div>
        <p class="font-14 fw-400 text-center mt-4">
          Don't have an account yet?
          <nuxt-link to="/auth/register" class="color-blue">
            Create an account</nuxt-link
          >
        </p>
      </form>
    </div>
  </section>
  <!-- End Cards -->
</template>
<script>
import HasError from 'vform/src/components/bootstrap5/HasError.vue'
import AlertError from 'vform/src/components/bootstrap4/AlertError.vue'
import Form from 'vform'
export default {
  data() {
    return {
      form: new Form({
        email: '',
        password: '',
      }),
    }
  },

  middleware: ['guest'],
  components: {
    HasError,
    AlertError,
  },
  methods: {
    async submit() {
      try {
        let response = await this.$auth.loginWith('local', { data: this.form })
        console.log(response)
      } catch (e) {
        this.form.errors.set(e.response.data.errors)
      }
    },
  },
}
</script>
