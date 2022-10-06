<template>
  <header :class="{ 'scrolled-nav': scrollPosition }">
    <div class="navContainer">
      <div class="branding">
        <RouterLink to="/"
          ><img src="../assets/images/ttlogo.svg" alt="tt logo"
        /></RouterLink>
        <RouterView />
      </div>
      <nav v-show="!mobile" class="navigation">
        <p class="link">
          Nice to see you <span>{{ name }}</span>
        </p>
        <button class="link" @click="logOut">LogOut</button>
      </nav>
      <div
        class="icon"
        @click="toggleMobileNav"
        v-show="mobile"
        :class="{ 'icon-active': mobileNav }"
      >
        <span class="bar"></span>
        <span class="bar"></span>
        <span class="bar"></span>
      </div>
      <transition name="mobile-nav">
        <nav v-show="mobileNav" class="dropdown-nav">
          <p class="link">
            Nice to see you <span>{{ name }}</span>
          </p>
          <button class="link" @click="logOut">LogOut</button>
        </nav>
      </transition>
    </div>
    <hr />
  </header>
</template>

<script setup>
import { ref, computed } from "vue";
import { useRouter } from "vue-router";
import { RouterLink, RouterView } from "vue-router";
import { useUserStore } from "../stores/user";

//constant to save a variable that will hold the use router method
const userStore = useUserStore();
// constant to save a variable that will get the user from store with a computed function imported from vue
const user = userStore.user;

// constant that calls user email from the useUSerStore
const userEmail = user.email;
// constant that saves the user email and cleans out the @client from the user
const name = userEmail.replace(/@.*$/, "");
// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();
const logOut = async () => {
  try {
    await userStore.signOut();
    redirect.push({ path: "/auth/login" });
  } catch (error) {
    errorMsg.value = `Error: ${error.message}`;
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
};

const scrollPosition = ref(null);
const mobile = ref(true);
const mobileNav = ref(false);
const windowWidth = ref(null);
</script>

<style scoped>
img {
  width: 50px;
  transition: 0.5s ease all;
}
header {
  background-color: #feecb7;
  z-index: 99;
  width: 100%;
  position: fixed; /*si scrolldown se queda*/
  transition: 0.5s ease all;
}
.navContainer {
  position: relative;
  display: flex;
  justify-content: space-between; /*meu*/
  align-content: center;
  align-items: center;
  width: 90%;
  margin: 0 auto;
  height: 80px; /*meu*/
  padding: 12px 0;
  transition: 0.5s ease all;
  @media (min-width: 1140px) {
    max-width: 1140px;
  }
}
.link {
  transition: 0.5s ease all;
}
.icon {
  display: flex;
  flex-direction: column;
  position: absolute;
  align-items: center;
  right: 24px;
  cursor: pointer;
}

.bar {
  width: 30px;
  height: 3px;
  background-color: #63a1a5;
  transition: 0.8s ease all;
  margin: 5px auto;
}
.icon-active {
  transform: rotate(180deg);
}
.navigation {
  display: flex;
  gap: 200px;
  align-items: center;
}

button {
  padding: 10px 30px;
  background: #72c1c1;
  border: 2px solid #406c6c;
  color: #6c4040;
  border-radius: 10px;
  text-align: center;
  text-decoration: none;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: 0.5s;
  box-shadow: 2px 2px 5px #999;
}
button:hover {
  background-color: #406c6c;
  border: 2px solid #72c1c1;
  color: #72c1c1;
  box-shadow: 2px 2px 5px #999;
}

hr {
  box-shadow: 2px 2px 5px #999;
}
</style>
