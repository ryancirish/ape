<template>
	<div id="entry_proposal" class="">
		<div class="w-70 center">
			<div v-if="disp">
				<h1>please <NuxtLink to="/login"> login </NuxtLink></h1>
			</div>

			<main v-if="!disp">
				<div class="tc w-100 mb4">
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

				<div class="w-100">
					<div class="center w-50-ns">
					  <label class="f3 b db mb2">proposal title</label>
					  <input id="" class="input-reset ba b--black-20 pa2 mb2 db w-100" type="text" v-model="title">

					  <label class="f3 b db mb2">proposal text</label>
					  <textarea class="db border-box hover-black w-100 measure ba b--black-20 pa2 br2 mb2" v-model="proposal"></textarea>

					  <label class="f3 b db mb2">proposal expiry (days)</label>
					  <input id="" class="input-reset ba b--black-20 pa2 mb2 db w-100" min="1" max="7" type="number" v-model="expIn">

					  <label class="f3 b db mb2">discussion channel link</label>
					  <input id="" class="input-reset ba b--black-20 pa2 mb2 db w-100" type="text" v-model="link">

					  <div class="mt3">
					  	<input class="tc b ph3 pv2 input-reset ba b--black bg-transparent grow pointer f6" value="submit proposal" @click="render" v-if="!renderMB">
					  </div>
					</div>

					<div class="w-40 center tc pt4">
						<MoneyButton
					      class="tc"
					      amount="0"
					      currency="BSV"
					      label="propose"
					      :script="script"
					      v-if="proposal && script"
					      @payment="handlePayment"
					    />
				    </div>				
				</div>
			</main>

		</div>
	</div>
</template>

<script>
	import axios from 'axios'
	import moment from 'moment'
	import BSV from 'bsv'
export default {

  name: 'propose',

  data () {
    return {
    	fetchedUser: false,
    	fetchedProfile: false,
    	isLoggedIn: false,
    	disp: null,
    	proposal: null,
    	link: null,
    	title: null,
    	expIn: null,
    	renderMB: false,
    	script: null
    }
  },

  methods: {
  	timeCheck(time) {
  		let elapsedDuration = moment.duration(moment().diff(time))
  		elapsedDuration = elapsedDuration.asMinutes()
  		if (elapsedDuration > 59) {
  			return true
  		} else {
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
  	},

	handlePayment(payment) {
      // need to write the tx info (include author) to the db
      // include the time as well via moment (that way you can calc expiry)
      this.man = payment
      this.$modal.show('complete')
    },

    async render() {
    	this.script = await this.inj()
    },

    inj() {
    	let prefix = '19HxigV4QyBv3tHpQVcUEQyq1pzZVdoAut'
    	let type = 'text/plain' //future support markdown cause this is ugly asf
    	//time to calc if expired, may be a couple ms different from the tx time
    	let now = moment().format() 
    	let n = `time: ${now} \r\n`
    	let author = `author: ${this.fetchedUser.data.attributes.name} \r\n`
    	let title = `title: ${this.title} \r\n`
    	let expiry = `expiry: ${this.expIn} days \r\n` //eventually could timestamp exact expiry idk

    	let man = title + author + n + expiry + this.proposal
    	
    	return bsv.Script.buildSafeDataOut([prefix, man, type]).toASM()
    }
  },

  mounted() {
  	console.log(BSV)
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
  },

  computed: {

  }
}
</script>

<style lang="css" scoped>
</style>