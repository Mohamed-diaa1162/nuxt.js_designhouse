<template>
  <li class="d-table w-100">
    <div class="stats-txt d-table-cell w-50">
      <a href="#" @click.prevent="likeDesign()" v-if="$auth.loggedIn">
        <template v-if="current_user_likes">
          <span>
            <i class="fas fa-thumbs-down fa-2x text-danger"></i>
          </span>
          UnLike
        </template>
        <template v-else>
          <span>
            <i class="fas fa-thumbs-up fa-2x text-success"></i>
          </span>
          Like
        </template>
      </a>
    </div>
    <div class="stats-num d-table-cell w-50 text-right">
      <a href="#">{{ design.likes_count }} Likes</a>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    design: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      current_user_likes: false,
    }
  },

  methods: {
    checkIfCurrentUserLikes() {
      this.$axios
        .get(`auth/Design/${this.design.id}/liked`)
        .then((res) => {
          console.log(res.data.liked)
          this.current_user_likes = res.data.liked
        })
        .catch((e) => console.log(e))
    },

    likeDesign() {
      this.$axios.post(`/auth/Design/${this.design.id}/like`).then((res) => {
        this.current_user_likes = !this.current_user_likes

        if (this.current_user_likes == true) {
          this.design.likes_count = this.design.likes_count + 1
        } else {
          this.design.likes_count = this.design.likes_count - 1
        }
      })
    },
  },

  mounted() {
    this.checkIfCurrentUserLikes()
  },
}
</script>
