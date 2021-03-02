<template>
	<h1>test</h1>
</template>

<script>
	import { MoneyButtonClient } from '@moneybutton/api-client'
export default {

  name: 'test',

  data () {
    return {

    }
  },

  methods: {
  	test() {
  		const client = new MoneyButtonClient(process.env.NUXT_ENV_CLIENT_ID)
  		client.requestAuthorization(
		  'auth.user_identity:read',
		  process.env.NUXT_ENV_CLIENT_REDIR
		)
		client.handleAuthorizationResponse()
  	}
  },

  mounted() {
  	this.test()
  }
}
</script>

<style lang="css" scoped>
</style>