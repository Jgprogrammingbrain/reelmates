<!-- src/App.vue -->
<template>
  <div id="app">
    <!-- Router view will handle all page rendering -->
    <router-view />
  </div>
</template>

<script>
import { onAuthStateChanged } from "firebase/auth";
import { auth } from "./firebaseConfig";
import { useRouter } from 'vue-router';

export default {
  name: "App",
  setup() {
    const router = useRouter();

    // Listen for authentication state changes
    onAuthStateChanged(auth, (user) => {
      if (user) {
        console.log("Auth state changed: User signed in");
        // If user is on login/register page, redirect to home
        if (router.currentRoute.value.path === '/login' || router.currentRoute.value.path === '/register') {
          router.push('/home');
        }
      } else {
        console.log("Auth state changed: No user signed in");
        // Only redirect to login if user is trying to access protected routes
        if (router.currentRoute.value.meta.requiresAuth) {
          router.push('/login');
        }
      }
    });
  }
};
</script>