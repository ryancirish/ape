<template>
	<div id="entry_proposal" class="debug-black">
		<div class="w-70 center">
			<div v-if="disp">
				<h1>please <NuxtLink to="/login"> login </NuxtLink>></h1>
			</div>


			<div class="tc">
				<h1 class="f1 tc dib mr4">new proposal</h1>
				<article class="mw5 center bg-white br3 pa3 pa4-ns mv3 ba b--black-10 dib v-mid">
				  <div class="tc">
				    <img src="https://c402277.ssl.cf1.rackcdn.com/photos/18331/images/carousel_small/Mountain_Gorilla_Uganda_WW267058.jpg?1576515791" class="br-100 h4 w4 dib ba b--black-05 pa2" title="Photo of a kitty staring at you">
				    <h1 class="f3 mb2">Mimi W.</h1>
				    <h2 class="f5 fw4 gray mt0">CCO (Chief Cat Officer)</h2>
				  </div>
				</article>
			</div>

		</div>
	</div>
</template>

<script>
export default {

  name: 'propose',

  data () {
    return {
    	fetchedUser: false,
    	fetchedProfile: false,
    	isLoggedIn: false,
    	disp: null
    }
  },

  methods: {
  	timeCheck(time) {
  		if (Math.round((performance.now() - time) / 1000) > 3600) {
  			return true
  		} else {
  			return false
  		}
  	}
  },

  mounted() {
  	if (!sessionStorage.getItem('mb_refresh_token')) {
  		console.log('no token, running auth...')
  		this.disp = true
  		
  	} else if (!sessionStorage.getItem('mb_timestart') || this.timeCheck(sessionStorage.getItem('mb_timestart'))) {
  		console.log('token expired, running auth...')
  		this.disp = true
  	} else {
  		console.log('token should be valid...')
  		
  		
  	}
  }
}
</script>

<style lang="css" scoped>
</style>