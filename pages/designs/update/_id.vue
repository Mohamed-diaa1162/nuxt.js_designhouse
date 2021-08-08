<template>
  <div>
    <!-- Start Hero -->
    <section class="hero text-center bg-secondary mb-4 text-white">
      <div class="container">
        <h1 class="font-28 fw-600 text-uppercase">Update design information</h1>
      </div>
    </section>
    <!-- End Hero -->

    <!-- Upload Shot -->
    <div class="upload-shot">
      <div class="container">
        <div class="row">
          <div class="col-md-6">
            <div class="card">
              <div class="card-body p-1">
                <!-- <img :src="design.images.original" class="w-100 mb-4" /> -->
              </div>
            </div>
          </div>
          <div class="col-md-6">
            <div class="card">
              <div class="card-body">
                <form class="auth-form" @submit.prevent="submit">
                  <AlertSuccess :form="form">
                    Design Successfuly Updated
                  </AlertSuccess>
                  <div class="form-group">
                    <base-input
                      :form="form"
                      field="title"
                      v-model="form.title"
                      placeholder="Write a title"
                    />
                  </div>
                  <div class="form-group">
                    <base-textarea
                      :form="form"
                      field="description"
                      v-model="form.description"
                      placeholder="Write a description"
                    />
                  </div>
                  <div class="form-group">
                    <client-only>
                      <better-input-tag
                        :tags="form.tags"
                        placeholder="Tags Separated By Commes"
                        on-paste-delimiter=","
                      ></better-input-tag>
                    </client-only>
                  </div>
                  <template v-if="teams.length">
                    <div class="form-group custom-control custom-checkbox">
                      <input
                        type="checkbox"
                        class="custom-control-input"
                        id="assign_to_team"
                        v-model="form.assign_to_team"
                      />
                      <label
                        for="assign_to_team"
                        value="false"
                        class="custom-control-label"
                      >
                        Assign to team
                      </label>
                    </div>
                    <div class="form-group">
                      <select
                        class="custom-select"
                        v-model="form.team"
                        :disabled="!form.assign_to_team"
                        :class="{
                          'is-invalid': form.errors.has('assign_to_team'),
                        }"
                      >
                        <option :value="null">Select a Team</option>
                        <option
                          v-for="team in teams"
                          :key="team.id"
                          :value="team.id"
                        >
                          {{ team.name }}
                          <!-- {{ team.id }} -->
                        </option>
                      </select>
                      <HasError :form="form" field="team" />
                    </div>
                  </template>
                  <div class="form-group custom-control custom-checkbox">
                    <input
                      type="checkbox"
                      class="custom-control-input"
                      id="is_live"
                      v-model="form.is_live"
                    />
                    <label
                      for="is_live"
                      value="true"
                      class="custom-control-label"
                    >
                      Publish this design
                    </label>
                  </div>
                  <div class="text-right">
                    <nuxt-link
                      class="mr-1 text-danger"
                      style="line-height: 5px"
                      to="/user/settings/designs"
                      >Cancel</nuxt-link
                    >
                    <base-button :loading="form.busy">
                      Update Design
                    </base-button>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- End Upload Shot -->
  </div>
</template>

<script>
import Form from 'vform'
import BetterInputTag from 'better-vue-input-tag'
import HasError from 'vform/src/components/bootstrap5/HasError.vue'
import AlertSuccess from 'vform/src/components/bootstrap4/AlertSuccess.vue'
export default {
  components: {
    BetterInputTag,
    HasError,
    AlertSuccess,
  },
  data() {
    return {
      form: new Form({
        title: '',
        description: '',
        is_live: false,
        tags: [],
        assign_to_team: false,
        team: null,
      }),
    }
  },

  async asyncData({ $axios, params, redirect, error }) {
    try {
      const design = await $axios.$get(`auth/Design/${params.id}/byUser`)
      const teams = await $axios.$get(`/auth/user/teams`)

      return { design: design.data, teams: [teams.data] }
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

  mounted() {
    if (this.design) {
      Object.keys(this.form).forEach((key) => {
        if (this.design.hasOwnProperty(key)) {
          this.form[key] = this.design[key]
        }
      })
      this.form.tags = this.design.tag_list.tags || []

      // console.log(this.design)
      if (this.design.team) {
        this.form.team = this.design.team.id
        this.form.assign_to_team = true
      } else {
        this.form.assign_to_team = false
      }
    }
    // console.log(this.$auth.user)
  },
  methods: {
    submit() {
      this.form
        .put(`auth/Design/${this.$route.params.id}`)
        .then((res) => {
          setTimeout(() => {
            this.$router.push('/user/settings/designs')
          }, 1000)
        })
        .catch((err) => console.log(err))
    },
  },

  middleware: ['auth'],
}
</script>
