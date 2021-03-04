<template>
	<div id="entry_success">
		<h1>{{ test }}</h1>
	</div>
</template>

<script>
	import axios from 'axios'
	import qs from 'qs'
	import { MoneyButtonClient } from '@moneybutton/api-client'
export default {

  name: 'test2',

  data () {
    return {
   	  test: ''
    }
  },

  async mounted() {
  	// need a check for token before sending req for token
  	let q = this.$route.query.code
  	let s = this.$route.query.state
  	let inj = {
  		grant_type: 'authorization_code',
  		client_id: process.env.NUXT_ENV_CLIENT_ID,
  		code: q,
  		redirect_uri: process.env.NUXT_ENV_CLIENT_REDIR
  	}

  	const config = {
	  headers: {
	    'Content-Type': 'application/x-www-form-urlencoded'
	  }
	}

	let z
	try {
		z = await axios.post('https://www.moneybutton.com/oauth/v1/token', qs.stringify(inj))	
	} catch(e) {
		console.log(e)
		// alert to login or somethin
	}

	if(z && z.status  == 200) {
		this.test = z.data
		sessionStorage.setItem('mb_refresh_token', z.data.refresh_token)
		sessionStorage.setItem('mb_access_token', z.data.access_token)
		sessionStorage.setItem('mb_expiry', z.data.expires_in)
		sessionStorage.setItem('mb_timestart', performance.now())
	} 
  	  	
  	if (sessionStorage.getItem('mb_refresh_token')) {
  		const AuthStr = 'Bearer '.concat(sessionStorage.getItem('mb_access_token'))
	  	axios.get('https://www.moneybutton.com/api/v1/auth/user_identity', { headers: { Authorization: AuthStr } })
	  	 .then(response => {
	  	     // If request is good...
	  	     this.test = response.data
	  	     console.log(response)

	  	  })
	  	 .catch((error) => {
	  	     console.log('error ' + error);
	  	  })
  	}
  }
}
</script>

<style lang="css" scoped>
</style>