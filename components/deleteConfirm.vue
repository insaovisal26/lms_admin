<template>
  <v-dialog v-model="dialog" max-width="500">
    <v-card>
      <v-card-title>"Are you sure you want to delete this?"</v-card-title>
      <v-card-text>
        <template v-if="route === 'posts'">
          <strong>{{ item.title }}</strong
          ><br />
          <v-icon small>mdi-account</v-icon> {{ item.userId }}<br />
          <v-icon small>mdi-text-subject</v-icon> {{ item.body }}
        </template>
        <template v-if="route === 'todos'">
          <strong>{{ item.title }}</strong
          ><br />
          <v-icon small>mdi-account</v-icon> {{ item.userId }}<br />
          <v-icon small>mdi-check</v-icon> {{ item.completed }}
        </template>
        <template v-if="route === 'users'">
          <strong>{{ item.name }}</strong
          ><br />
          <v-icon small>mdi-pound-box</v-icon> {{ item.id }}<br />
          <v-icon small>mdi-email</v-icon> {{ item.email }}
        </template>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn text @click="cancel()">
        Cancel
        </v-btn>
        <v-btn color="red" @click="confirm()">
        Delete
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      resolve: null,
      reject: null,
      route: null,
      item: null,
    }
  },
  methods: {
    open(item = null) {
      if (item !== null) {
        this.route = this.$route.name.split(/-|_/)[0]
        this.item = item
      }
      this.dialog = true
      return new Promise((resolve, reject) => {
        this.resolve = resolve
        this.reject = reject
      })
    },
    confirm() {
      this.resolve(true)
      this.dialog = false
    },
    cancel() {
      this.resolve(false)
      this.dialog = false
    },
  },
}
</script>
