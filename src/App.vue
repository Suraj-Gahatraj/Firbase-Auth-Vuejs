<script>
import { EmailAuthProvider } from 'firebase/auth';
import { ref } from 'vue'
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import {
  getRedirectResult,
  signInWithRedirect,
  signOut,
  createUserWithEmailAndPassword,
  signInWithEmailAndPassword,
 
} from 'firebase/auth';
import { useCurrentUser, useFirebaseAuth } from 'vuefire';


export default {
  setup() {
    //const { createUserWithEmailAndPassword } = useAuth();
    const auth = useFirebaseAuth()
  //  const user = useCurrentUser()
    const email = ref('');
    const password = ref('');

    const signUp = async () => {
      
        console.log(email.value)
        createUserWithEmailAndPassword(auth,email.value, password.value)
        .then((userCredential) => {
            const user = userCredential.user;
            console.log("user created Successfully")
            console.log(user)
          }).catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
          });
        
        
      
    };

    const signIn = async () => {
        console.log(email.value)
         signInWithEmailAndPassword(auth,email.value, password.value)
         .then((userCredential) => {
            const user = userCredential.user;
            console.log("user signed in success fully")
            console.log(user)
          }).catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
          });

};

const signOutUser=async()=>
{
  signOut(auth);
}


    return {
      email,
      password,
      signUp,
      signIn,
      signOut
    };
  }
}

</script>

<template>
  <div>
    <h2>Sign Up</h2>
    <form @submit.prevent="signUp">
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="email" required>
      </div>
      <div>
        <label for="password">Password:</label>
        <input type="password" id="password" v-model="password" required>
      </div>
      <button type="submit">Sign Up</button>
    </form>
  </div>

  <div>
    <h2>Sign IN</h2>
    <form @submit.prevent="signIn">
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email_sign_in" v-model="email" required>
      </div>
      <div>
        <label for="password">Password:</label>
        <input type="password" id="password_sign_in" v-model="password" required>
      </div>
      <button type="submit">Sign In</button>
    </form>
  </div>

  <div>
    <button @click="signOutUser">Sign Out</button>
  </div>
  <p v-if="user">Hello {{ user.providerData.displayName }}</p>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
