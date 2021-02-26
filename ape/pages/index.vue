<template>
<div class="helvetica">
    <div class="helvetica vh-100 h-100 cover bg-left bg-center-l" style="background-image: url('https://media.giphy.com/media/SDeVLvFCqFsSA/giphy-downsized-large.gif')">
      <div class="bg-black-50 pb5 pb6-m pb7-l h-100">
        <nav class="dt w-100 mw8 center"> 
          <div class="dtc w2 v-mid pa3">
            <a href="/" class="f1 dib w2 h2 pa1 grow-large no-underline">
              🦍
            </a>
          </div>
          <div class="dtc v-mid tr pa3">
            <NuxtLink class="f4 fw4 hover-white no-underline white-90 dn dib-l pv2 ph3 grow dib" to="/blog">blog</NuxtLink>
            <NuxtLink class="f4 fw4 hover-white no-underline white-90 dn dib-l pv2 ph3 grow dib" to="/about" >about</NuxtLink>
            <a class="f4 fw4 hover-white no-underline white-90 dib ml2 pv2 ph3 ba dib" href="#beta" >join us</a>
          </div>
        </nav> 
        <div class="tc-l mt4 mt5-m mt6-l ph3">
          <h1 class="f-subheadline-ns f1 white mb0 lh-title ttu">ape.capital</h1>
          <h2 class="fw4 f1-ns f2 white mt3 mb4">Put your money where your mouth is</h2>
          <a class="f3 no-underline grow dib v-mid bg-blue white ba b--blue ph3 pv2 mb3" href="#beta">join us</a>
          <span class="dib v-mid ph3 white-70 mb3">or</span>
          <NuxtLink class="f3 no-underline grow dib v-mid white ba b--white ph3 pv2 mb3" to="/about">learn more</NuxtLink>

          <div class="w-50 center mt4-ns mt5">
            <a href="https://twitter.com/DaoGorilla" target="_blank" class="grow dib-ns mr4-ns"><img src="../static/twitter.png" class="h3"></a>
            <a href="https://twetch.app/u/34859" target="_blank" class="grow dib-ns v-top-ns"><img src="../static/twetch.png" class="h2 mt3-ns mt4"></a>
          </div>
        </div>
      </div>
    </div>


    <div id="beta" class="h-auto">
      <div id="sign" class="mt6 pb6">
        <h1 class="f-subheadline tc"> Sign Up </h1>
        <div class="pa4-l v-mid">
          <form class="bg-light-red mw7 center pa4 br2-ns ba b--black-10">
            <fieldset class="cf bn ma0 pa0">
              <legend class="pa2 f3-ns mb3 black-80 tc b">Enter your email for updates</legend>
              <div class="cf">
                <label class="clip" for="email-address">Email Address</label>
                <input class="f6 f5-l input-reset bn fl black-80 bg-white pa3 lh-solid w-100 w-75-m w-80-l br2-ns br--left-ns" v-model="email" placeholder="Your Email Address" type="text" name="email-address" value="" id="email-address">
                <a class="f6 f5-l button-reset fl pv3 tc bn bg-animate bg-black-70 hover-bg-black white pointer w-100 w-25-m w-20-l br2-ns br--right-ns"  @click="mailSubmission()">ape it</a>
              </div>
            </fieldset>
          </form>
        </div>
      </div>
    </div>
</div>
</template>

<script>
  import firebase from "firebase"
export default {
  data() {
    return {
      email: null,
      reg: /^(([^<>()\]\\.,;:\s@"]+(\.[^<>()\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/
    }
  },

  methods:{
    async mailSubmission() {
      if (this.email == null || this.email == '') {
        alert('please enter a valid email address')
      } else if(!this.reg.test(this.email)) {
        alert('please enter a valid email address')
      } else {
        let dt = firebase.firestore.Timestamp.fromDate(new Date())
        this.$fire.firestore
          .collection('beta')
          .add({date: dt, email: this.email})
          .then(() => {
            console.log('signup complete: ' + this.email)
            this.email = ''
            this.$router.push({ path: '/thanks'})            
          })
          .catch(e => {
            console.log(e)
            alert(e.message)
          })
      }
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
