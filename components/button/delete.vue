<template>
  <v-tooltip bottom>
    <template v-slot:activator="{ on, attrs }">
      <v-btn
        color="red"
        x-small
        fab
        dark
        v-bind="attrs"
        v-on="on"
        @click.stop="removeConfirm()"
      >
        <v-icon :small="small">mdi-delete</v-icon>
      </v-btn>
      <delete-confirm ref="deleteConfirm"></delete-confirm>
    </template>
    <span>Delete</span>
  </v-tooltip>
</template>

<script>
import deleteConfirm from '@/components/deleteConfirm.vue'
export default {
  components: {
    deleteConfirm,
  },
  props: {
    path: {
      type: String,
      required: true,
    },
    id: {
      type: Number,
      required: true,
    },
    item: {
      type: Object,
      required: false,
      default: null,
    },
    small: {
      type: Boolean,
      required: false,
      default: false,
    },
  },
  methods: {
    async removeConfirm() {
      if (await this.$refs.deleteConfirm.open(this.item)) {
        this.remove()
      } else {
        // Do something in case of "cancel"
      }
    },
    async remove() {
      await this.$store.dispatch(`${this.path}s/delete`, this.id)
      this.$router.push(`/admin/${this.path}`)
    },
  },
}
</script>
