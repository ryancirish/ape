<template>
	<div id="entry_proposals">
		<div class="w-90 center h-auto pb6">
			<h1 class="tc">proposals</h1>
			<div v-for="proposal in man" v-if="man" class="measure-wide center mt5">
				<h1>{{ proposal.title }}</h1>
				<h3>{{ proposal.createdAtf }}</h3>
				<!-- need expirty here -->
				<div class="mt3">
					<a :href="proposal.link" v-if="proposal.link" target="_blank">chat link</a>
					<a :href="'https://bitcoinfiles.org/t/' + proposal.txid" target="_blank">proposal file</a>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import moment from 'moment'
export default {

  name: 'proposals',

  data () {
    return {
    	man: []  	
    }
  },

  methods: {
  	source () {
  		return this.$fire.firestore.collection('proposals').get()
  			.then(r => {
  				r.docs.forEach(e => {
  					this.man.push({createdAtf: moment(e.data().createdAt).format('LLLL') ,...e.data()})
  				})
  				
  			})
  		
  	}
  },

  mounted() {
  	this.source()
  },


}
</script>

<style lang="css" scoped>
</style>