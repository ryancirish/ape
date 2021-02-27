<template>
	<div id="entry_success">
		<h1>{{ test }}</h1>
	</div>
</template>

<script>
	import axios from 'axios'
	import { MoneyButtonClient } from '@moneybutton/api-client'
export default {

  name: 'test2',

  data () {
    return {
   	  test: ''
    }
  },

  mounted() {
  	const AuthStr = 'Bearer '.concat(localStorage.getItem('mb_js_client:oauth_access_token'))
  	
  	if (localStorage.getItem('mb_js_client:oauth_access_token')) {
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