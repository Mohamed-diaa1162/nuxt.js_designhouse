<template>
  <!-- Start Header -->
  <header class="fixed-topx">
    <nav class="navbar navbar-expand-lg navbar-dark align-items-center">
      <nuxt-link
        class="navbar-brand text-uppercase fw-500"
        to="/"
        title="Project Name"
        >DesignHouse</nuxt-link
      >
      <button
        class="navbar-toggler mr-auto"
        type="button"
        data-toggle="collapse"
        data-target="#navbar"
        aria-controls="navbar"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbar">
        <ul class="navbar-nav font-14 fw-300">
          <li class="nav-item active">
            <nuxt-link class="nav-link" to="/designs/search" title="Shots"
              >Designs</nuxt-link
            >
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#" title="Designers"> Designers </a>
          </li>
        </ul>
        <div class="header-search">
          <form action="" method="">
            <div class="form-group">
              <input
                type="text"
                autocomplete="OFF"
                name="search"
                class="form-control text-white font-14 fw-300"
                placeholder="Search and hit enter..."
              />
              <div class="search-icon text-white">
                <i class="fas fa-search"></i>
              </div>
            </div>
          </form>
        </div>
        <div
          class="upload-shot white-path font-14 fw-500 text-uppercase mr-auto"
        >
          <nuxt-link to="/designs/upload" class="primary-bg-color text-white">
            <i class="fas fa-cloud-upload-alt"></i> Upload
          </nuxt-link>
        </div>
      </div>

      <!-- Before Login -->
      <ul
        class="before-login font-14 fw-300 text-uppercase"
        v-if="!$auth.loggedIn"
      >
        <li>
          <!-- <a href="#"></a> -->
          <nuxt-link no-prefetch to="/auth/register">Sign up</nuxt-link>
        </li>
        <li>
          <nuxt-link no-prefetch to="/auth/login">Sign in</nuxt-link>
        </li>
      </ul>
      <!-- End Before Login -->

      <!-- After Login -->

      <ul class="author-page white-path" v-else>
        <!-- Profile Dropdown -->
        <li class="dropdown">
          <a
            href="#"
            class="dropdown-toggle text-white"
            id="userDropdown"
            role="button"
            data-toggle="dropdown"
            aria-haspopup="true"
            aria-expanded="false"
          >
            <img
              class="user-thumb"
              :src="$auth.user.photo_url"
              alt="Neba Funwi-Gabga"
            />
            <div class="usr-info">
              <span class="user-name font-14 fw-500">{{
                $auth.user.name
              }}</span>
              <span class="user-deg font-10 fw-300">{{
                $auth.user.tagline
              }}</span>
              <span class="down-chevron">
                <i class="fas fa-angle-down"></i>
              </span>
            </div>
          </a>
          <div
            class="dropdown-menu user-dropdown font-14 fw-500"
            aria-labelledby="userDropdown"
          >
            <div class="dropdown-title-group font-12 fw-500">
              <span class="dropdown-title text-uppercase">Your Account</span>
            </div>
            <nuxt-link
              class="dropdown-item mt-28"
              to="/user/settings/dashboard"
              title="Dashboard"
            >
              <i class="fas fa-tachometer-alt"></i>
              Dashboard
            </nuxt-link>
            <nuxt-link
              class="dropdown-item"
              to="/user/settings/profile"
              title="Profile"
            >
              <i class="fa fa-user"></i>
              Profile
            </nuxt-link>
            <nuxt-link
              class="dropdown-item"
              to="/user/settings/designs"
              title="Designs"
            >
              <i class="fa fa-user"></i>
              Designs
            </nuxt-link>
            <nuxt-link
              class="dropdown-item"
              to="/user/settings"
              title="Setting"
            >
              <i class="fa fa-cogs"></i>
              Setting
            </nuxt-link>
            <div class="dropdown-divider"></div>
            <a
              class="dropdown-item"
              href="#"
              title="Sign Out"
              @click.prevent="logout"
            >
              <i class="fa fa-lock"></i>
              Sign Out
            </a>
          </div>
        </li>
        <!-- End Profile Dropdown -->
      </ul>
      <!-- End After Login -->
    </nav>
  </header>
  <!-- End Header -->
</template>

<script>
export default {
  methods: {
    logout() {
      this.$auth.logout()
    },
  },
}
</script>
