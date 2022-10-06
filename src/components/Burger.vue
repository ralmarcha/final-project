<template>
  <header :class="{ 'scrolled-nav': scrolledNav }">
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
      <div class="icon" @click="toggleMobileNav" v-show="mobile">
        <span class="bar" :class="{ 'icon-active': mobileNav }"></span>
        <span class="bar" :class="{ 'icon-active': mobileNav }"></span>
        <span class="bar" :class="{ 'icon-active': mobileNav }"></span>
      </div>
      <transition name="mobile-nav">
        <nav v-show="mobileNav" class="dropdown-nav">
          <p class="link-drop">
            Nice to see you <span>{{ name }}</span>
          </p>
          <button class="link-drop" @click="logOut">LogOut</button>
        </nav>
      </transition>
    </div>
    <hr />
  </header>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import { useRouter } from "vue-router";
import { RouterLink, RouterView } from "vue-router";
import { useUserStore } from "../stores/user";
import { onMounted } from "vue";

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

const scrolledNav = ref(null);
const mobile = ref(true);
const mobileNav = ref(false);
const windowWidth = ref(null);

const toggleMobileNav = () => {
  mobileNav.value = !mobileNav.value;
};
computed(() => {
  window.addEventListener("resize", checkScreen.value);
  checkScreen();
});
function checkScreen() {
  windowWidth.value = window.innerWidth;
  if (windowWidth.value <= 750) {
    mobile.value = true;
    return;
  }
  mobile.value = false;
  mobileNav.value = false;
  return;
}
</script>

<style scoped>
img {
  width: 50px;
  transition: 0.5s ease all;
}
header {
  background-color: #feebb3;
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
  background-color: #dd7210;
  transition: 0.8s ease all;
  margin: 5px auto;
}
.icon-active {
  transform: rotateY(180deg);
}

.dropdown-nav {
  display: flex;
  flex-direction: column;
  position: fixed;
  width: 100%;
  max-width: 300px;
  height: 100%;
  background-color: white;
  top: 0;
  left: 0;
}
.link-drop {
  margin-left: 0;
}
.navigation {
  display: flex;
  gap: 200px;
  align-items: center;
}

.mobile-nav-enter-active,
.mobile-nav-leave-active {
  transition: 1s ease all;
}
.mobile-nav-enter-from,
.mobile-nav-leave-to {
  transform: translateX(-350px);
}
.mobile-nav-enter-to {
  transform: translateX(0);
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
<div class="displayOnMobile" id="userIcon" @click="openUserMenu">
    <span class="bar" :class="{ 'icon-active': mobileNav }"></span>
    <span class="bar" :class="{ 'icon-active': mobileNav }"></span>
    <span class="bar" :class="{ 'icon-active': mobileNav }"></span>
  </div>

  <div
    class="dontDisplayOnMobile"
    id="navigation"
    v-if="useUserStore().user != null"
  >
    <p class="link">
      Nice to see you <span>{{ name }}</span>
    </p>
    <button type="submit" @click="logOut">LogOut</button>
  </div>
</div>
<transition name="mobile-nav">
  <div
    class="displayOnMobile displayNone"
    id="mobileUserMenu"
    v-if="useUserStore().user != null"
  >
    <p class="link-drop">
      Nice to see you <span>{{ name }}</span>
    </p>
    <p class="link-drop" @click="logOut">LogOut</p>
  </div>
</transition>