<template>
  <section class="authentication">
    <div class="auth-body">
      <h1 class="text-uppercase fw-500 mb-4 text-center font-22">
        Resend Verification Email
      </h1>
      <form class="auth-form" @submit.prevent="submit">
        <AlertError :form="form" v-if="form.errors.has('Verify')">
          {{ form.errors.get('Verify') }}
          <nuxt-link to="/verification/resend">
            Resend Verification Email
          </nuxt-link>
        </AlertError>
        <AlertSuccess
          :form="form"
          message="The Verification Email Has Been Resend "
        />
        <div class="form-group">
          <input
            type="text"
            name="email"
            :form="form"
            class="form-control form-control-lg font-14 fw-300"
            placeholder="Email"
            v-model.trim="form.email"
            :class="{ 'is-invalid': form.errors.has('email') }"
          />
          <HasError :form="form" field="email" />
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
            Resend
          </button>
        </div>
      </form>
    </div>
  </section>
</template>

<script>
import HasError from 'vform/src/components/bootstrap5/HasError.vue'
import Button from 'vform/src/components/bootstrap5/Button.vue'
import AlertError from 'vform/src/components/bootstrap4/AlertError.vue'
import AlertSuccess from 'vform/src/components/bootstrap4/AlertSuccess.vue'
import Form from 'vform'
export default {
  data() {
    return {
      form: new Form({
        email: '',
      }),
    }
  },
  components: {
    HasError,
    Button,
    AlertSuccess,
    AlertError,
  },
  methods: {
    submit() {
      this.form
        .post('/guest/verification/resend')
        .then((res) => this.form.reset())
        .catch((e) => console.log(e))
    },
  },
}
</script>
