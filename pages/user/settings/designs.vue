<template>
  <div class="setting-block">
    <div class="setting-title font-16 fw-500">Designs</div>
    <div class="setting-body white-bg-color">
      <table class="table table-striped table-dark">
        <thead>
          <tr>
            <th scope="col"></th>
            <th scope="col">Title</th>
            <th scope="col">Status</th>
            <th scope="col">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="design in designs.data" :key="design.id">
            <th>
              <div v-if="design.images">
                <img :src="design.images.thumbnail" width="100" />
              </div>
            </th>
            <td>{{ design.title }}</td>
            <td>{{ design.is_liv ? 'Puplished' : 'Only You' }}</td>
            <td>
              <nuxt-link
                class="btn btn-primary"
                :to="`/designs/update/${design.id}`"
                >Edit</nuxt-link
              >
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
export default {
  middleware: 'auth',

  data() {
    return {
      designs: [],
    }
  },
  methods: {
    async fetchUserDesigns() {
      const { data } = await this.$axios.get(
        `/Users/${this.$auth.user.id}/designs`
      )
      this.designs = data
    },
  },

  created() {
    this.fetchUserDesigns()
  },
}
</script>
