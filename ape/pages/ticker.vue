<template>
	<div id="entry_ticker">
		<h1 v-if="data">
			ticker
		</h1>
		<div v-else class="measure-wide tc center mt6">
			<NuxtLink class="f-headline-ns" to="/login">please login</NuxtLink>
		</div>
	</div>
</template>

<script>
	import axios from 'axios'
export default {

  name: 'ticker',

  data () {
    return {
    	test: null,
    	data: null
    }
  },

  mounted() {
  	if (sessionStorage.getItem('mb_refresh_token')) {
  		this.data = true
  		const AuthStr = 'Bearer '.concat(sessionStorage.getItem('mb_access_token'))
	  	axios.get('https://www.moneybutton.com/api/v2/me/wallets/38436/balances', { headers: { Authorization: AuthStr } })
	  	 .then(response => {
	  	     // If request is good...
	  	     if (response.status == 200) {
	  	     	this.test = true
	  	     	console.log('here')
	  	     	this.data = response.data
	  	     }
	  	  })
	  	 .catch((error) => {
	  	     console.log('error ' + error);
	  	  })
  	} else {
  		console.log('test')
  	}
  }
}
</script>

<style lang="css" scoped>
</style>