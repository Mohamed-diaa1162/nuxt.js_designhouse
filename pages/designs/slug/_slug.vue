<template>
  <section class="post-details mt-4 pb-5">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-md-9">
          <div class="row">
            <!-- LEFT -->
            <div class="col-md-8">
              <!-- Single Image -->
              <div class="post-detail">
                <div class="single-img">
                  <img :src="design.images.large" alt="Image" />
                </div>
              </div>
              <!-- End Single Image -->
              <!-- Design Detail Text -->
              <div class="desing-text font-16 fw-400 pb-3 pt-2">
                <p>
                  {{ design.description }}
                </p>
              </div>
              <!-- End Design Detail Text -->
              <!-- Design Comments -->
              <div class="design-comments mt-3">
                <h1 class="font-16 fw-300 mb-4">
                  <strong class="fw-500"
                    >{{ design.comments_count }} Comments</strong
                  >
                </h1>
                <div v-if="comments">
                  <base-comments
                    v-for="comment in comments"
                    :key="comment.id"
                    :comment="comment"
                    @deleted="handelDelete"
                  ></base-comments>
                </div>
              </div>

              <template v-if="$auth.loggedIn">
                <form action="" @submit.prevent="save">
                  <base-textarea
                    :form="form"
                    :row="2"
                    field="body"
                    v-model="form.body"
                    placeholder="Write Your Comment"
                  />
                  <div class="mt-2 text-right">
                    <base-button :loading="form.busy" size="sm"
                      >Post a Comment</base-button
                    >
                  </div>
                </form>
              </template>

              <!--/ END COMMENTS-->
            </div>

            <!-- RIGHT -->
            <div class="col-md-4">
              <div class="post-detail-sidebar">
                <!-- Designer info -->
                <div class="modal-user-meta white-bg-color">
                  <a class="float-left" href="#" title="Neba">
                    <img :src="design.user.photo_url" />
                  </a>
                  <div class="modal-user-detail">
                    <h1 class="font-13 fw-500">
                      <a href="#">{{ design.user.name }} </a>
                    </h1>
                    <p class="font-12 fw-300 mt-1">
                      <span class="shot-by">{{ design.user.tagline }}</span>
                    </p>
                    <p class="font-12 fw-300 mt-1">
                      {{ design.created_dates.created_at_human }}
                    </p>
                  </div>
                </div>
                <!-- End Designer info -->
                <!-- Designer Design Info -->
                <ul class="details-side-meta font-14 fw-400">
                  <design-like :design="design"></design-like>

                  <li class="d-table w-100">
                    <div class="stats-txt d-table-cell w-100">
                      <a href="#"> More from {{ design.user.name }} </a>
                    </div>
                  </li>
                </ul>
                <!-- End Designer Design Info -->
                <!-- Designer More Designs -->
                <div class="more-designs-outer pb-3">
                  <ul class="more-designs row">
                    <li
                      class="col-md-6"
                      v-for="user_design in user_designs"
                      :key="user_design.id"
                    >
                      <nuxt-link :to="`/designs/slug/${user_design.slug}`">
                        <img
                          class="w-100"
                          :src="user_design.images.thumbnail"
                          alt="Image"
                        />
                      </nuxt-link>
                    </li>
                  </ul>
                </div>
                <!-- End Designer More Designs -->
                <!-- Designs Tags -->
                <div class="designs-tag-outer mt-3">
                  <h2 class="font-16 fw-500 mb-2">Tags</h2>
                  <div
                    class="designs-tag font-14 fw-300"
                    v-if="design.tag_list"
                  >
                    <nuxt-link
                      v-for="tag in design.tag_list.tags"
                      :key="tag"
                      :to="`/tags/${tag}`"
                    >
                      {{ tag }}
                    </nuxt-link>
                  </div>
                </div>
                <!-- End Designs Tags -->
              </div>
            </div>
            <!--/ END RIGHT-->
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Form from 'vform'
import DesignLike from '~/components/DesignLike.vue'
export default {
  components: DesignLike,
  data() {
    return {
      form: new Form({
        body: '',
      }),
      user_designs: [],
    }
  },
  async asyncData({ params, $axios, error }) {
    try {
      const response = await $axios.$get(`/Designs/slug/${params.slug}`)
      return { design: response.data, comments: response.data.comments }
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

  methods: {
    handelDelete(id) {
      this.comments = this.comments.filter((c) => c.id !== id)
    },

    save() {
      this.form
        .post(`/auth/Design/${this.design.id}/comments`)
        .then((res) => {
          this.form.reset()
          this.comments = [...this.comments, res.data.data]
        })
        .catch((e) => console.log(e))
    },
  },

  created() {
    this.$axios
      .get(`/Users/${this.design.user.id}/designs`)
      .then((res) => (this.user_designs = res.data.data))
      .catch((e) => console.log(e))
  },
}
</script>
