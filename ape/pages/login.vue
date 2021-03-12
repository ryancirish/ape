<template>
	<div class="measure-wide center tc">
		<div v-if="state">
			<h1>you are already logged in.</h1>
			<div class="f3">
				<NuxtLink class="db mb4" to="/propose"> new proposal </NuxtLink>
				<NuxtLink class="db mb4" to="/proposals"> see proposals </NuxtLink>
				<NuxtLink class="db mb4" to="/transact"> transact </NuxtLink>
			</div>
		</div>
	</div>
</template>

<script>
	import { MoneyButtonClient } from '@moneybutton/api-client'
	import moment from 'moment'
export default {

  name: 'test',

  data () {
    return {
    	state: false
    }
  },

  methods: {
  	async auth() {
  		const client = new MoneyButtonClient(process.env.NUXT_ENV_CLIENT_ID)
  		client.requestAuthorization(
		  'auth.user_identity:read',
		  process.env.NUXT_ENV_CLIENT_REDIR
		)
		await client.handleAuthorizationResponse()
    const refresh_token = client.getRefreshToken()
    client.authorizeWithAuthFlowResponse(
          receivedQueryParameters,
          expectedStateValue,
          redirectUri
    )
    sessionStorage.setItem('_refresh_token', refresh_token)
    console.log(client)
  	},

  	timeCheck(time) {
  		let elapsedDuration = moment.duration(moment().diff(time))
  		elapsedDuration = elapsedDuration.asMinutes()
  		if (elapsedDuration > 59) {
  			return true
  		} else {
  			return false
  		}
  	}
  },

  mounted() {
  	if (!sessionStorage.getItem('mb_refresh_token')) {
  		console.log('no token, running auth...')
  		this.auth()
  	} else if (!sessionStorage.getItem('mb_timestart') || this.timeCheck(sessionStorage.getItem('mb_timestart'))) {
  		console.log('token expired, running auth...')
  		this.auth()
  	} else {
  		console.log('token should be valid...')
  		//probably dont hit the api here because it's a waste of a call
  		this.state = true
  	}
  }
}
</script>

<style lang="css" scoped>
</style>