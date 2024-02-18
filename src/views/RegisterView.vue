<template>
  <h1> Creat an Account</h1>
  <p><input type="text" placeholder="Email" v-model="email"/></p>
  <p><input type="password" placeholder="Password" v-model="password"/></p>
  <p v-if="errorMessage">{{ errorMessage }}</p>
  <p><button @click="register">Submit</button></p>
  <p><button @click="signInWithGooggle">Sign In With Google</button></p>
</template>

<script setup>
import { ref } from "vue";
import {
  getAuth,
  createUserWithEmailAndPassword,
  GoogleAuthProvider,
  signInWithPopup
} from "firebase/auth";
import { useRouter } from "vue-router";

const email = ref("");
const password = ref("");
const router = useRouter();
const errorMessage = ref();
const register = () => {
  createUserWithEmailAndPassword(getAuth(), email.value, password.value)
  .then(() => {
    router.push({ path: '/feed'});
  })
  .catch((error) => {
    console.log(error.code);
    switch(error.code) {
      case "auth/invalid-email":
        errorMessage.value = "Invalid Email";
        break;
      case "auth/weak-password":
        errorMessage.value = "Please provide a stronger/longer password";
        break;
      case "auth/missing-password":
        errorMessage.value = "Please provide a password";
        break;
      case "auth/email-already-in-use":
        errorMessage.value = "Email already in use";
        break;
      default: 
        errorMessage.value = "Error was not handled for this. Might be a server issue";
        break;
    }
  });
};
const signInWithGooggle = () => {
  const provider = new GoogleAuthProvider();
  signInWithPopup(getAuth(), provider)
  .then(() => {
    router.push('/feed');
  })
  .catch(() => {
    errorMessage.value = "Sign in Failed!"
  }); 
};
</script>