<template>
  <div>
    <!-- Start Hero -->
    <section
      class="hero text-center bg-light shadow-sm text-blue"
      v-if="!$auth.loggedIn"
    >
      <div class="container">
        <h1 class="font-28 fw-600 text-uppercase">What are you working on?</h1>
        <p class="font-16 fw-400">
          DesignHouse is where designers get inspired.
        </p>
        <a class="mt-4 btn btn-danger font-16 fw-400" href="#"
          >Sign up for Free</a
        >
      </div>
    </section>

    <section class="cards-block">
      <div class="container">
        <div class="row">
          <base-designs
            v-for="desgin in designs"
            :key="desgin.id"
            :design="desgin"
          />
        </div>
      </div>
    </section>
    <!-- End Hero -->
  </div>
</template>

<script>
export default {
  async asyncData({ params, $axios, error }) {
    try {
      const response = await $axios.$get(`/Designs`)
      return { designs: response.data }
    } catch (err) {
      if (err.response.status == 404) {
        error({ statusCode: 404, message: 'Desgin Not Found' })
      } else if (err.response.status == 401) {
        redirect('/auth/login')
      } else {
        error({ statusCode: 500, message: 'Internal Server Error' })
      }
      console.log(err)
    }
  },
}
</script>
