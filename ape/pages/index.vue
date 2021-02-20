<template>
<div class="helvetica">
    <div class="helvetica vh-100 h-100 cover bg-left bg-center-l" style="background-image: url(https://media.giphy.com/media/SDeVLvFCqFsSA/giphy.gif)">
      <div class="bg-black-50 pb5 pb6-m pb7-l h-100">
        <nav class="dt w-100 mw8 center"> 
          <div class="dtc w2 v-mid pa3">
            <a href="/" class="f1 dib w2 h2 pa1 grow-large no-underline">
              🦍
            </a>
          </div>
          <div class="dtc v-mid tr pa3">
            <NuxtLink class="f4 fw4 hover-white no-underline white-90 dn dib-l pv2 ph3" to="/blog">blog</NuxtLink>
            <a class="f4 fw4 hover-white no-underline white-90 dn dib-l pv2 ph3" href="#about" >about</a>
            <a class="f4 fw4 hover-white no-underline white-90 dib ml2 pv2 ph3 ba" href="#beta" >join us</a>
          </div>
        </nav> 
        <div class="tc-l mt4 mt5-m mt6-l ph3">
          <h1 class="f-subheadline white mb0 lh-title">ape.capital</h1>
          <h2 class="fw4 f1 white mt3 mb4">Put your money where your mouth is</h2>
          <a class="f3 no-underline grow dib v-mid bg-blue white ba b--blue ph3 pv2 mb3" href="#beta">join us</a>
          <span class="dib v-mid ph3 white-70 mb3">or</span>
          <a class="f3 no-underline grow dib v-mid white ba b--white ph3 pv2 mb3" href="#about">learn more</a>
        </div>
      </div>
    </div>

    <div class="h-auto w-80 center pt4" id="about">
      <h1 class="f-subheadline tc"> about </h1>
      <p class="f2"> Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
      consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
      cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
      proident, sunt in culpa qui officia deserunt mollit anim id est laborum. </p>
      <p class="f2"> Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
      consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
      cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
      proident, sunt in culpa qui officia deserunt mollit anim id est laborum. </p>
    </div>

    <div id="beta">
      <div class="pa4-l">
        <form class="bg-light-red mw7 center pa4 br2-ns ba b--black-10">
          <fieldset class="cf bn ma0 pa0">
            <legend class="pa2 f3-ns mb3 black-80 tc">Sign up for our beta</legend>
            <div class="cf">
              <label class="clip" for="email-address">Email Address</label>
              <input class="f6 f5-l input-reset bn fl black-80 bg-white pa3 lh-solid w-100 w-75-m w-80-l br2-ns br--left-ns" placeholder="Your Email Address" type="text" name="email-address" value="" id="email-address">
              <input class="f6 f5-l button-reset fl pv3 tc bn bg-animate bg-black-70 hover-bg-black white pointer w-100 w-25-m w-20-l br2-ns br--right-ns" type="submit" value="ape it">
            </div>
          </fieldset>
        </form>
      </div>
    </div>
</div>
</template>

<script>
export default {}
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
