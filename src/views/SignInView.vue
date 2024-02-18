<template>
  <h1> Sign in to your Account</h1>
  <p><input type="text" placeholder="Email" v-model="email"/></p>
  <p><input type="password" placeholder="Password" v-model="password"/></p>
  <p v-if="errorMessage">{{ errorMessage }}</p>
  <p><button @click="register">Submit</button></p>
  <p><button @click="signInWithGooggle">Sign In With Google</button></p>
</template>

<script setup>
import { ref } from "vue";
import {getAuth, 
  signInWithEmailAndPassword,
  GoogleAuthProvider,
  signInWithPopup 
} from "firebase/auth";
import { useRouter } from "vue-router";

const email = ref("");
const password = ref("");
const router = useRouter();
const errorMessage = ref();
const register = () => {
  signInWithEmailAndPassword(getAuth(), email.value, password.value)
  .then(() => {
    console.log("Successfully signed in!");
    router.push({ path: '/feed'});
  })
  .catch((error) => {
    switch(error.code) {
      case "auth/invalid-email":
        errorMessage.value = "Invalid Email";
        break;
      case "auth/user-not-found":
        errorMessage.value = "No account with that email was found";
        break;
      case "auth/wrong-passowrd":
        errorMessage.value = "Wrong Password";
        break;
      default: 
        errorMessage.value = "Email or Password was incorrect";
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