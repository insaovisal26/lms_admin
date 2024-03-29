<template>
  <div>
    <p>
      <v-card>
        <v-card-actions>
          <back-btn />
          <v-spacer />
          <create-btn path="/admin/listing" />
          <!-- <reload-btn path="listings" /> -->
        </v-card-actions>
      </v-card>
    </p>
    <p>
      <v-card>
        <v-card-title>
          Property lists
          <v-spacer />
          <filter-btn @click="filter.visible = !filter.visible" />
        </v-card-title>
        <v-container v-if="filter.visible === true">
          <v-row>
            <v-spacer />
            <v-col>
              <v-text-field
                v-model="filter.search"
                append-icon="mdi-magnify"
                label="Search"
                single-line
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
        <v-data-table
          class="table-cursor"
          :headers="headers"
          :items="listings"
          :items-per-page="10"
          :sort-by.sync="sortBy"
          :sort-desc.sync="sortDesc"
          :search="filter.search"
          @click:row="(item) => show(item.id)"
        >
          <template v-slot:[`item.userId`]="{ item }">
            <v-chip @click.stop="user(item.userId)">{{ item.userId }}</v-chip>
          </template>
          <template v-slot:[`item.is_sale`]="{ item }">
            <v-simple-checkbox
              v-model="item.is_sale"
              disabled
            ></v-simple-checkbox>
          </template>
          <template v-slot:[`item.is_rent`]="{ item }">
            <v-simple-checkbox
              v-model="item.is_rent"
              disabled
            ></v-simple-checkbox>
          </template>
          <template v-slot:[`item.sale_price`]="{ item }">
              ${{ item.sale_price }}
          </template>
          <template v-slot:[`item.rent_price`]="{ item }">
              {{ item.rent_price }}
          </template>
          <template v-slot:[`item.width`]="{ item }">
              {{ item.width }}m²
          </template>
          <template v-slot:[`item.length`]="{ item }">
              {{ item.length }}m²
          </template>
          <template v-slot:[`item.area`]="{ item }">
              {{ item.area }}m²
          </template>
          <template v-slot:[`item.actions`]="{ item }">
            <show-btn :id="item.id" path="listing" small />
            <edit-btn :id="item.id" path="listing" small />
            <delete-btn :id="item.id" path="listing" :item="item" small />
          </template>
        </v-data-table>
      </v-card>
    </p>
    <delete-confirm ref="deleteConfirm"></delete-confirm>
  </div>
</template>

<script>
import deleteConfirm from '@/components/deleteConfirm.vue'
import backBtn from '@/components/button/back'
import reloadBtn from '@/components/button/reload'
import deleteBtn from '@/components/button/delete'
import filterBtn from '@/components/button/filter'
import createBtn from '@/components/button/create'
import editBtn from '@/components/button/edit'
import showBtn from '@/components/button/show'
import { mapGetters } from 'vuex'



export default {
  middleware: 'auth',
  components: {
    deleteConfirm,
    backBtn,
    reloadBtn,
    deleteBtn,
    filterBtn,
    createBtn,
    editBtn,
    showBtn
  },
  async fetch({ store }) {
    if (store.state.listings.list.length === 0) {
      await store.dispatch('listings/fetchList')
    }
  },
  data() {
    return {
      sortBy: "id",
      sortDesc: false,
      filter: {
        visible: false,
        search: '',
      },
      headers: [
        { text: 'ID', align: 'left', value: 'id' },
        { text: 'Property Owner', align: 'left', value: 'property_owner' },
        { text: 'Type', align: 'left', value: 'type' },
        { text: 'Sale', align: 'left', value: 'is_sale' },
        { text: 'Rent', align: 'left', value: 'is_rent' },
        { text: 'Sale Price', align: 'left', value: 'sale_price' },
        { text: 'Rent Price', align: 'left', value: 'rent_price' },
        { text: 'Width', align: 'left', value: 'width' },
        { text: 'Length', align: 'left', value: 'length' },
        { text: 'Area', align: 'Area', value: 'area' },
        { text: 'Address', align: 'left', value: 'address' },
        { text: 'Actions', align: 'left', value: 'actions' },
      ],
    }
  },
  computed: {
    listings() {
      return this.$store.getters['listings/list']
    },
    ...mapGetters(['loggedInUser'])
  },
  methods: {
    show(id) {
      this.$router.push(`/admin/listing/${id}`)
    },
    user(id) {
      this.$router.push(`admin/users/${id}`)
    },
    getIsSale (is_sale) {
        if (is_sale == 'True') return 'Yes'
        else return 'No'
    },
    getIsRent (is_rent) {
        if (is_rent == 'True') return 'Yes'
        else return 'No'
    },
    getColor (status) {
        if (status == 'True') return 'blue'
        else if (status == "In Progress") return 'red'
        else return 'green'
    },
  },
}
</script>

<style>
.table-cursor tbody tr:hover {
  cursor: pointer;
}
</style>
