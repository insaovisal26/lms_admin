<template>
  <div>
    <p>
      <v-card>
        <v-card-actions>
          <back-btn />
          <list-btn path="/admin/user" />
          <v-spacer />
          <delete-btn :id="user.id" path="user" />
          <!-- <reload-btn :id="user.id" path="users" @reloaded="reloaded" /> -->
        </v-card-actions>
      </v-card>
    </p>
    <p>
      <v-card>
        <v-card-title>edit</v-card-title>
        <user-form :user="user" @save="save" />
      </v-card>
    </p>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import backBtn from '@/components/button/back'
import listBtn from '@/components/button/list'
import deleteBtn from '@/components/button/delete'
import reloadBtn from '@/components/button/reload'
import userForm from '@/components/form/user'

export default {
  components: {
    backBtn,
    listBtn,
    deleteBtn,
    reloadBtn,
    userForm,
  },
  // async fetch({ store }) {
  //   if (store.state.posts.list.length === 0) {
  //     await store.dispatch('posts/fetchList')
  //   }
  // },
  data() {
    return {
      user: {
        id: -1,
      },
    }
  },
  computed: {
    ...mapGetters({
      getById: 'users/getById',
    }),
  },
  // mounted() {
  //   this.post = Object.assign({}, this.getById(this.$route.params.id))
  // },
  async mounted() {
    if (this.$store.state.users.list.length === 0) {
      await this.fetchList()
    }
    this.user = Object.assign({}, this.getById(this.$route.params.id))
  },
  methods: {
    ...mapActions('users', ['fetchList']),
    reloaded(item) {
      this.user = item
    },
    async save() {
      await this.$store.dispatch('users/update', this.user)
      this.$router.push(`/admin/user`)
    },
  },
}
</script>
