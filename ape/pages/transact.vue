<template>
	<div id="entry_test" class="helvetica">
		<div class="tc pt4">
			<NuxtLink class="tc center f-subheadline-ns f1 mb0 no-underline hover-bg-orange pa1" to="/">🦍</NuxtLink>
		</div>
		<div class="measure center">
			<h1 class="tc">get ape tokens</h1>
			<p class="tc">amount (BSV): </p>
			<div class="tc">
				<input class="center w-20" type="text" v-model="amount">
			</div>
			<div class="w-40 center tc pt4">
				<MoneyButton
			      to="38436@moneybutton.com"
			      class="tc"
			      :amount="amount"
			      currency="BSV"
			      label="get ape tokens"
			      v-if="validAmount"
			      @payment="handlePayment"
			    />
		    </div>
	    </div>
		
		<modal name="complete" width="70%" height="auto" @closed="redir">
			<h1 class="pl3">payment details</h1>
			<div v-if="man" class="pb5 f3 ph4">
				<p class="pb2">time: {{ man.createdAt }}</p>
				<p class="pb2">amount: {{ man.amount }} {{ man.currency }}</p>
				<p class="pb2">fee amount USD: {{ man.feeAmountUsd }}</p>
				<p class="pb2">fee amount sata: {{ man.feeAmountSatoshis }}</p>
				<p class="pb2">status: {{ man.status }}</p>
				<p class="mw8 break">txid: {{ man.txid }}</p>
			</div>
		</modal>
	</div>
</template>

<script>
	import axios from 'axios'
	import { MoneyButtonClient } from '@moneybutton/api-client'
	
export default {

  name: 'test',

  data () {
    return {
    	amount: '',
    	man: null
    }
  },

  mounted() {
  	//this.test()
  },

  methods: {

	handlePayment(payment) {
      // handle payment
      /*fields to display back to user
      amount
      createdAt
      currency
      feeAmountUSD
      feeAmountSatoshis
      status
      txid
      */
      this.man = payment
      this.$modal.show('complete')
      

    },

    isNumeric(value) {
          return /^\d+$/.test(value);
    },

    redir() {
    	console.log('complete, redirecting...')
    	this.$router.push({ path: '/thankyou' })
    }
  },

  computed: {

  	validAmount() {
  		if (this.amount) {
	  		if (parseFloat(this.amount)) {
	  			return parseFloat(this.amount)	
	  		}
  		}
  	}
  }


}
</script>

<style lang="css" scoped>
	.break {
		word-wrap: break-word;
	}
</style>