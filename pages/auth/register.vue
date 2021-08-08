<template>
  <!-- Section Cards -->
  <section class="authentication">
    <div class="auth-body">
      <h1 class="text-uppercase fw-500 mb-4 text-center font-22">Register</h1>
      <form class="auth-form" @submit.prevent="submit">
        <AlertSuccess
          :form="form"
          message="We have Sent you  an email to activate your account"
        />
        <div class="form-group">
          <base-input
            v-model.trim="form.name"
            placeholder="Name"
            :form="form"
            field="name"
          />
        </div>
        <div class="form-group">
          <base-input
            v-model.trim="form.username"
            placeholder="User Name"
            :form="form"
            field="username"
          />
        </div>
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
        <div class="form-group">
          <base-input
            v-model.trim="form.password_confirmation"
            placeholder="password Confirmation"
            inputType="password"
            :form="form"
            field="password_confirmation"
          />
        </div>
        <div class="text-right">
          <base-button :loading="this.form.busy">Register</base-button>
        </div>
        <p class="font-14 fw-400 text-center mt-4">
          Already have an account?
          <nuxt-link to="/auth/login" class="color-blue"> Login</nuxt-link>
        </p>
      </form>
    </div>
  </section>
  <!-- End Cards -->
</template>
<script>
// import { Button, HasError, AlertError } from 'vform/src/components/bootstrap4'
import HasError from 'vform/src/components/bootstrap5/HasError.vue'
import AlertSuccess from 'vform/src/components/bootstrap4/AlertSuccess.vue'
import Form from 'vform'
export default {
  data() {
    return {
      form: new Form({
        name: '',
        username: '',
        email: '',
        password: '',
        password_confirmation: '',
      }),
    }
  },
  components: {
    HasError,
    AlertSuccess,
  },

  middleware: ['guest'],

  methods: {
    submit() {
      this.form
        .post('/guest/Register')
        .then((res) => {
          this.form.reset()
          console.log(res)
        })
        .catch((e) => {
          console.log(e)
        })
    },
  },
}
</script>
