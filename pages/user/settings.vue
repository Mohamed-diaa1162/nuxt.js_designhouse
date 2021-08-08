<template>
  <!-- Start User Account Setting -->
  <div class="user-account-setting">
    <div class="container">
      <div class="row">
        <!-- LEFT -->
        <div class="col-sm-3" id="myScrollspy">
          <div class="list-group">
            <nuxt-link
              v-for="link in links"
              :key="link.route"
              class="list-group-item list-group-item-action"
              :to="link.route"
              >{{ link.title }}</nuxt-link
            >
          </div>
        </div>

        <!-- RIGHT -->
        <div class="col-sm-9">
          <router-view></router-view>
        </div>
        <!-- END RIGHT -->
      </div>
    </div>
    <!-- END CONTAINER -->
  </div>
</template>
<script>
export default {
  data() {
    return {
      links: [
        { title: 'Dashboard', route: '/user/settings/dashboard' },
        { title: 'Profile', route: '/user/settings/profile' },
        { title: 'Designs', route: '/user/settings/designs' },
      ],
    }
  },
  middleware: 'auth',
}
</script>
