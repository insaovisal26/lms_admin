<template>
  <v-tooltip bottom>
    <template v-slot:activator="{ on, attrs }">
      <v-btn 
        color="primary mx-1" 
        x-small
        fab
        v-bind="attrs" 
        v-on="on" 
        @click.stop="edit()">
        <v-icon :small="small">mdi-pencil</v-icon>
      </v-btn>
    </template>
    <span>Edit</span>
  </v-tooltip>
</template>

<script>
export default {
  props: {
    path: {
      type: String,
      required: true,
    },
    id: {
      type: Number,
      required: true,
    },
    small: {
      type: Boolean,
      required: false,
      default: false,
    },
  },
  methods: {
    edit() {
      this.$router.push(this.path + `/${this.id}/edit`)
    },
  },
}
</script>
