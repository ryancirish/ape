<template>
	<div id="entry_proposal" class="">		
		<div class="w-90 center h-auto pb6">
			<div v-if="disp" class="tc">
				<h1>please <NuxtLink to="/login"> login </NuxtLink></h1>
			</div>

			<main v-if="!disp">
				<div class="tc w-100 mb4">
					<h1 class="f1 tc dib-ns mr4">new proposal</h1>
					<article class="mw5 center bg-white br3 pa3 pa4-ns mv3 ba b--black-10 dib-ns v-mid">
					  <div class="tc" v-if="fetchedUser">
					    <img src="https://c402277.ssl.cf1.rackcdn.com/photos/18331/images/carousel_small/Mountain_Gorilla_Uganda_WW267058.jpg?1576515791" class="br-100 h4 w4 dib ba b--black-05 pa2" title="Photo of a kitty staring at you">
					    <div class="">
					    	<h1 class="f3 mb2">{{ fetchedUser.data.attributes.name }}</h1>
					    	<h2 class="f5 fw4 gray mt0">id: {{ fetchedUser.data.id }}</h2>
					    </div>
					  </div>
					  <div v-else>
					  	<img src="../static/loading.gif">
					  </div>
					</article>
				</div>

				<div class="w-100">
					<div class="center w-50-ns">
					  <label class="f3 b db mb2">proposal title</label>
					  <input class="input-reset ba b--black-20 pa2 mb2 db w-100" type="text" v-model="title">

					  <label class="f3 b db mb2">proposal text</label>
					  <textarea class="db border-box hover-black w-100 measure ba b--black-20 pa2 br2 mb2" v-model="proposal"></textarea>
            <select name="filetype" v-model="typeSelect">
              <option value="text/plain" selected>plaintext</option>
              <option value="text/markdown">markdown</option>                            
            </select>

					  <label class="f3 b db mb2 mt4">proposal expiry (days)</label>
					  <input class="input-reset ba b--black-20 pa2 mb2 db w-100" min="1" max="7" type="number" v-model="expIn">

					  <label class="f3 b db mb2">discussion channel link (<a href="https://chat.ape.capital">chat.ape.capital</a>)</label>
					  <input class="input-reset ba b--black-20 pa2 mb2 db w-100" type="text" v-model="link">

					  <div class="mt3">
					  	<button class="tc b ph3 pv2 input-reset ba b--black bg-transparent grow pointer f6" value="" @click="inj" v-if="visButton"> submit proposal </button>

					  	<!-- <button class="tc b ph3 pv2 input-reset ba b--black bg-transparent grow pointer f6" value="" @click="inj" v-if="!visButton"> click when tx is complete </button> -->
					  </div>
					</div>

					<div class="w-40 center tc pt4">
						<div id="mb"></div>
				    </div>				
				</div>
			</main>

		</div>
	</div>
</template>

<script>
	import axios from 'axios'
	import moment from 'moment'
  import firebase from "firebase"
	var bsv = require('bsv')

export default {

  name: 'propose',

  data () {
    return {
    	fetchedUser: false,
    	fetchedProfile: false,
    	isLoggedIn: false,
    	disp: null,
    	proposal: null,
    	link: '',
    	title: null,
    	expIn: null,
    	renderMB: false,
    	script: null,
    	visButton: true,
    	metadata: null,
    	man: null,
      typeSelect: 'text/plain'
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
      
      // write the tx & metadata to firebase key, 
    },

    redir() {
    	console.log('complete, redirecting...')
    	this.$router.push({ path: '/apesanity' })
    },

    runModal() {
      this.$modal.show('complete')
    },

    inj() {
    	let prefix = '19HxigV4QyBv3tHpQVcUEQyq1pzZVdoAut'
    	let type = this.typeSelect 
    	//time to calc if expired, may be a couple ms different from the tx time
    	let now = moment().format() 
    	let n = `time: ${now}  `
    	let author = `author: ${this.fetchedUser.data.attributes.name}  `
    	let title = `title: ${this.title}  `
    	let expiry = `expiry: ${this.expIn} days  ` //eventually could timestamp exact expiry idk

    	let man = title + '\n' + author + '\n' + n  + '\n' + expiry + '\n' + this.proposal
    	this.metadata = {
    		now: now,
    		time: n,
    		author: author,
    		title: title,
    		expiry: expiry
    	}

    	const script = bsv.Script.buildSafeDataOut([prefix, man, type]).toASM()
    	
    	const div = document.getElementById('mb')
      let test = ''
    	moneyButton.render(div, {
    		outputs: [{
    			script,
    			amount: '0',
    			currency: 'BSV',
          successMessage: 'debug'
    		}],
        label: 'write prop to chain',
        onPayment: (arg) => { 
          // console.log('onPayment', arg)
          // args to get from the tx, need to commit to firebase right now
          localStorage.setItem('prop_createdAt', arg.createdAt)
          localStorage.setItem('prop_amount', arg.amount)
          localStorage.setItem('prop_currency', arg.currency)
          localStorage.setItem('prop_feeAmountUsd', arg.feeAmountUsd)
          localStorage.setItem('prop_feeAmountSatoshis', arg.feeAmountSatoshis)
          localStorage.setItem('prop_feeAmountSq', arg.amount)
          localStorage.setItem('prop_feeAmountTxid', arg.txid)
          localStorage.setItem('prop_feeAmountStatus', arg.status)
          localStorage.setItem('prop_feeAmountUid', arg.userId)
          localStorage.setItem('prop_link', this.link)
          localStorage.setItem('prop_title', this.title)
          
          const pkg = `createdAt: ${arg.createdAt}` + "\n" + `amount: ${arg.amount} ${arg.currency}` + "\n"
          + `fee amount usd: ${arg.feeAmountUsd}` + "\n" + `fee amount usd: ${arg.feeAmountUsd}` + "\n" + `amount: ${arg.amount}` + "\n"  + `txid: ${arg.txid}` + "\n"  + `status: ${arg.status}`
          alert(pkg)
          this.pusher(arg)                            
        }
    	})

      //this.pusher()
      
    },

    pusher(arg) {
      let txid = localStorage.getItem('prop_feeAmountTxid')
      this.$fire.firestore
        .collection('proposals')
        .doc(txid)
        .set({
          createdAt: localStorage.getItem('prop_createdAt'),
          amount: localStorage.getItem('prop_amount'),
          currency: localStorage.getItem('prop_currency'),
          feeAmountUsd: localStorage.getItem('prop_feeAmountUsd'),
          feeAmountSatoshis: localStorage.getItem('prop_feeAmountSatoshis'),
          amount: localStorage.getItem('prop_feeAmountSq'),
          txid: txid,
          status: localStorage.getItem('prop_feeAmountStatus'),
          uid: localStorage.getItem('prop_feeAmountUid'),
          link: this.link,
          title: this.title
        })
        .then(() => {
          console.log('added')
          this.email = ''
          this.$router.push('chainTx')                          
        })
        .catch(e => {
          console.log(e)
          // alert(e.message)
          // catch all log here
        })      
    }
  },

  mounted() {
    
    //console.log(this.$fire.firestore.collection('proposals').add({test: 'test'}))
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

  watch: {
    name(newName, oldName) {
      console.log(newName, oldName)
    }
  },

  computed: {

  }
}
</script>

<style lang="css" scoped>
</style>