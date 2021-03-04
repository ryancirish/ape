<template>
	<div id="entry_proposal" class="debug-black">
		<div class="w-70 center">
			<div v-if="disp">
				<h1>please <NuxtLink to="/login"> login </NuxtLink>></h1>
			</div>


			<div class="tc">
				<h1 class="f1 tc dib-ns mr4">new proposal</h1>
				<article class="mw5 center bg-white br3 pa3 pa4-ns mv3 ba b--black-10 dib-ns v-mid">
				  <div class="tc" v-if="fetchedUser">
				    <img src="https://c402277.ssl.cf1.rackcdn.com/photos/18331/images/carousel_small/Mountain_Gorilla_Uganda_WW267058.jpg?1576515791" class="br-100 h4 w4 dib ba b--black-05 pa2" title="Photo of a kitty staring at you">
				    <h1 class="f3 mb2">{{ fetchedUser.data.attributes.name }}</h1>
				    <h2 class="f5 fw4 gray mt0">id: {{ fetchedUser.data.id }}</h2>
				  </div>
				  <div v-else>
				  	<img src="../static/loading.gif">
				  </div>
				</article>
			</div>

		</div>
	</div>
</template>

<script>
	import axios from 'axios'
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
  			console.log('true')
  			return true
  		} else {
  			console.log('false: ', Math.round((performance.now() - time) / 1000))
  			return false
  		}
  	},

  	getProfile(id, auth) {
  		// yes im doing this all for a pfp but whatever
  		// wait I have to refactor to add another permission to the scope
  		let u = `https://www.moneybutton.com/api/v1/users/${id}/profile`
  		axios.get(u, { headers: { Authorization: AuthStr } })
  		  .then(response => {
  		  	this.fetchedProfile = response.data
  		  })
  		  .catch(err => {
  		  	console.log('error: ' + error)
  		  })
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
  		// could prob put this in vue datastore but meh do it on demand
  		const AuthStr = 'Bearer '.concat(sessionStorage.getItem('mb_access_token'))

  		//convert to try/catch, make it functional blah blah blah
	  	axios.get('https://www.moneybutton.com/api/v1/auth/user_identity', { headers: { Authorization: AuthStr } })
	  	 .then(response => {
	  	     // If request is good...
	  	     this.fetchedUser = response.data
	  	     //this.getProfile(response.data.id, AuthStr) wait until scope is fixed
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