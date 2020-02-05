<template>
  <b-container>
    <h2 class="title">FUNDING</h2>

    <b-table
      :items="funding"
      :fields="fields"
      :sort-by.sync="sortBy"
      :sort-desc.sync="sortDesc"
      stacked="sm"
    >
      <template slot="address" scope="funding">
        <nuxt-link :to="'/investment/' + funding.item.id">{{ funding.item.address }}</nuxt-link>
      </template>
    </b-table>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      stuff: { thing: 'my thang' },
      sortBy: 'id',
      sortDesc: false,
      fields: [
        // { key: 'id', sortable: true },
        { key: 'address', sortable: true },
        { key: 'purpose', sortable: true },
        { key: 'loan_amount_dollars', sortable: true },
        { key: 'rate', sortable: true },
        { key: 'expected_term_months', sortable: true },
        { key: 'fully_funded', sortable: true },
        { key: 'created_on', sortable: true }
      ]
    }
  },
  async asyncData({ $axios }) {
    let funding = await $axios.get('/api/funding')
    return {
      funding: funding.data
    }
  }
}
</script>
