<template>
  <div>
    <!-- Start Filters -->
    <section class="filters-block shadow-sm">
      <div class="container">
        <form @submit.prevent="search">
          <div
            class="filters d-flex justify-content-between align-items-center"
          >
            <ul class="filters-dropdown">
              <li class="dropdown">
                <select
                  v-model="filters.orderBy"
                  class="custom-select"
                  style="width: 200px"
                  @change="search"
                >
                  <option value="latest">Latest First</option>
                  <option value="likes">Most Liked First</option>
                </select>
              </li>
            </ul>

            <div class="d-flex align-items-center">
              <div class="custom-control mr-3 custom-checkbox">
                <input
                  v-model="filters.has_comments"
                  type="checkbox"
                  class="custom-control-input"
                  id="hasComments"
                  true-value="1"
                  false-value="0"
                />
                <label class="custom-control-label" for="hasComments"
                  >Has Comments</label
                >
              </div>

              <div class="custom-control custom-checkbox mr-3">
                <input
                  type="checkbox"
                  class="custom-control-input"
                  id="hasTeam"
                  v-model="filters.has_team"
                  true-value="1"
                  false-value="0"
                />
                <label class="custom-control-label" for="hasTeam"
                  >By Team</label
                >
              </div>

              <div>
                <div class="input-group mb-0">
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Search..."
                    v-model="filters.q"
                  />
                  <div class="input-group-append">
                    <button
                      :disabled="searching"
                      class="btn rounded primary-bg-color text-white"
                      type="submit"
                    >
                      <span v-if="searching == true">
                        <i class="fas fa-spinner fa-spin"></i>
                      </span>
                      Search
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>
    </section>
    <!-- End Filters -->

    <!-- Section Cards -->
    <section class="cards-block">
      <div class="container">
        <div class="row">
          <base-designs
            v-for="design in designs"
            :key="design.id"
            :design="design"
          ></base-designs>
        </div>
        <!-- END ROW -->
      </div>
      <!--/ End Container -->
    </section>
    <!-- End Cards -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      designs: [],
      filters: {
        has_team: 0,
        has_comments: 0,
        q: '',
        orderBy: 'likes',
      },
      searching: false,
    }
  },
  computed: {
    queryString() {
      return Object.keys(this.filters)
        .map((k) => `${k}=${this.filters[k]}`)
        .join('&')
    },
  },

  methods: {
    search() {
      this.searching = true
      this.$axios
        .$get(`/search/designs?${this.queryString}`)
        .then((res) => (this.designs = res.data))
        .catch((err) => console.log(err))
        .finally(() => (this.searching = false))
    },
  },

  beforeMount() {
    this.search()
  },
}
</script>
