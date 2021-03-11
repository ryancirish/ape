<template>
	<div id="entry_chaintx">
		<div class="center">
			<h1 class="tc f1">proposal</h1>

			<div class="tc f3">
				<NuxtLink to="/proposals" class="">see all proposals</NuxtLink>
			</div>

			<div v-if="man && txid" class="center w-80-ns">
				<article class="pa3 pa5-ns w-90 center" data-name="slab-stat-small">
				  <h3 class="f6 ttu tracked pb3">meta</h3>
				  <div class="cf">
				    <dl class="fl fn-l w-50 dib-l w-auto-l lh-title mr5-l">
				      <dd class="f6 fw4 ml0">created</dd>
				      <dd class="f3 fw6 ml0">{{ man.createdAt }}</dd>
				    </dl>
				    <dl class="fl fn-l w-50 dib-l w-auto-l lh-title mr5-l">
				      <dd class="f6 fw4 ml0">amount</dd>
				      <dd class="f3 fw6 ml0">{{ man.amount }} {{ man.currency }}</dd>
				    </dl>
				    <dl class="fl fn-l w-50 dib-l w-auto-l lh-title mr5-l">
				      <dd class="f6 fw4 ml0">fee amount (usd)</dd>
				      <dd class="f3 fw6 ml0">{{ man.feeAmountUsd }}</dd>
				    </dl>
				    <dl class="fl fn-l w-50 dib-l w-auto-l lh-title mr5-l">
					  <dd class="f6 fw4 ml0">fee amount (sats)</dd>
				      <dd class="f3 fw6 ml0">{{ man.feeAmountSatoshis }}</dd>
				    </dl>
				    <dl class="fl fn-l w-50 dib-l w-auto-l lh-title mr5-l">
				      <dd class="f6 fw4 ml0">txid</dd>				      
				    </dl>
				    <p class="f3 fw6 ml0 word-wrap" id="txid">{{ man.txid }}</p>
				  </div>
				</article>
				<iframe :src="txid" width="100%" height="700"></iframe>
			</div>
		</div>
	</div>
</template>

<script>
	import moment from 'moment'
export default {

  name: 'chainTx',

  data () {
    return {
    	man: null,
    	txid: null
    }
  },

  mounted() {
  	this.man = {
  		createdAt: moment(localStorage.getItem('prop_createdAt')).format('LLLL'),
  		amount: localStorage.getItem('prop_amount'),
        currency: localStorage.getItem('prop_currency'),
        feeAmountUsd: localStorage.getItem('prop_feeAmountUsd'),
        feeAmountSatoshis: localStorage.getItem('prop_feeAmountSatoshis'),
        feeAmount: localStorage.getItem('prop_feeAmountSq'),
        txid: localStorage.getItem('prop_feeAmountTxid'),
        status: localStorage.getItem('prop_feeAmountStatus'),
        userId: localStorage.getItem('prop_feeAmountUid'),
  	}

  	this.txid = `https://bitcoinfiles.org/t/${this.man.txid}`

  	//
  }
}
</script>

<style lang="css" scoped>
	#txid {
		overflow-wrap: break-word;	
	}
	
</style>