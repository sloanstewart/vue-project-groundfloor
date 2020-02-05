<template>
  <b-container>
    <h1 class="title">LOAN</h1>
    <p>Address: {{ investment.address }}</p>
    <p>Purpose: {{ investment.purpose }}</p>

    <section>
      <b-form @submit="onSubmit">
        <b-form-group label="Investment Amount" label-for="investment_amount">
          <b-input-group>
            <b-input-group-prepend>
              <span class="input-group-text">$</span>
            </b-input-group-prepend>
            <b-form-input
              id="investment_amount"
              v-model="investment_amount"
              :state="hasInvest"
              type="number"
              name="investment_amount"
            />
          </b-input-group>
        </b-form-group>

        <b-button :disabled="!hasInvest" type="submit" variant="primary">Invest</b-button>
      </b-form>
    </section>

    <section class="investment_funds">
      <h3 class="subtitle">Loan Funds</h3>
      <b-table v-if="funds.length > 0" :fields="fields" :items="funds">placeholder</b-table>
      <b v-else span>No funds found</b>
    </section>

    <p>
      <b-button type="button" vairant="success" @click="goBack">Back</b-button>
    </p>
  </b-container>
</template>
<script>
export default {
  data() {
    return {
      id: this.$route.params.id,
      investment_amount: 0,
      fields: ['id', 'amount', 'created_on']
    }
  },
  computed: {
    hasInvest() {
      return this.investment_amount > 99
    }
  },
  async asyncData({ $axios, params }) {
    let investment = await $axios.get(`/api/investment/${params.id}`)
    let funds = await $axios.get(`/api/investment/${params.id}/funds`)
    return {
      investment: investment.data,
      funds: funds.data
    }
  },
  methods: {
    async onSubmit(ev) {
      ev.preventDefault()
      if (!this.hasInvest) {
        return
      }
      let { id, investment_amount } = this
      let data = {
        investment_id: id,
        amount: investment_amount
      }
      console.log(data)
      let invest = await this.$axios({
        method: 'post',
        url: '/api/funding',
        data
      })
      this.$router.go()
    },
    goBack() {
      this.$router.back()
    }
  }
}
</script>
