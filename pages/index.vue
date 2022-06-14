<template>
  <div>
    <h2 v-if="user">Signed in user: {{ user }}</h2>
    <div id="firebaseui-auth-container"></div>
    <div id="loader">Loading...</div>
  </div>
</template>

<script>
import firebase from 'firebase/compat/app'
import * as firebaseui from 'firebaseui'
import firebaseConfig from '../firebaseConfig'
import 'firebaseui/dist/firebaseui.css'
firebase.initializeApp(firebaseConfig)
const ui = new firebaseui.auth.AuthUI(firebase.auth())

export default {
  name: 'IndexPage',
  data() {
    return {
      user: '',
      isSignedIn: false,
    }
  },
  mounted() {
    ui.start('#firebaseui-auth-container', {
      signInFlow: 'popup',
      // signInSuccessUrl: 'http://localhost:3001/protect',
      signInOptions: [
        firebase.auth.GoogleAuthProvider.PROVIDER_ID,
        firebase.auth.FacebookAuthProvider.PROVIDER_ID,
        firebase.auth.GithubAuthProvider.PROVIDER_ID,
      ],
      callbacks: {
        signInSuccessWithAuthResult: (authResult) => {
          this.user = authResult.user.displayName || 'new User'
          console.log(authResult)
          this.isSignedIn = true

          return false
        },
        uiShown() {
          document.getElementById('loader').style.display = 'none'
        }
      }
    })
  },
}
</script>
