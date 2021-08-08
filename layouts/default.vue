<template>
  <div>
    <SideNavigation></SideNavigation>
    <section class="wrapper">
      <Nuxt />
    </section>
    <SideFooter></SideFooter>
  </div>
</template>

<script>
import SideNavigation from './partials/navigation.vue'
import SideFooter from './partials/footer.vue'

export default {
  components: {
    SideNavigation,
    SideFooter,
  },
}
</script>
