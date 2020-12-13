<template>
  <div>
    uid:{{ user.uid }}
  </div>
</template>

<script>
import firebase from "firebase";

export default {
  data() {
    return {
      user: {},
      authLock: false,
    };
  },

  mounted: function() {
    console.log('mount')
    this.initFirebase();
  },

  methods: {
    initFirebase: function() {
      if (!firebase.apps.length) {
        firebase.initializeApp({
          apiKey: "AIzaSyDG5kF5p-cCnEgNDhZ6lm1njaAf7M-7rE4",
          authDomain: "auth-test-20201213.firebaseapp.com",
          projectId: "auth-test-20201213.firebaseapp.com",
        });
      }
      firebase.auth().onAuthStateChanged((user) => {
        if (user) {
          this.user = user
          console.log("authed");
        } else {
          if (this.authLock) {
            console.log("auth cancel");
            return;
          }
          this.authLock = true;
          console.log("authing");
          firebase.auth().signInAnonymously()
        }
      });
    },
  }
};
</script>

