<template>
  <div class="container">
    <div class="left">
      <div class="div1">
        <img class="icon" src="../assets/images/TT.png" alt="tt logo" />
      </div>
      <div class="div2">
        <img
          class="logo"
          src="../assets/images/triptick.png"
          alt="triptick logo"
        />
        <h2>Your Trip Begins Here!</h2>
      </div>
      <div class="div3">
        <form @submit.prevent="signIn" class="div3">
          <input
            class="inputEmail"
            type="email"
            placeholder="E-mail"
            v-model="email"
            id="email"
          />
          <br />
          <div class="pass">
            <input
              class="passwordInputType"
              :type="passwordFieldType"
              onpaste="return false"
              placeholder="Password"
              v-model="password"
              id="password"
            />
            <i
              :class="passwordIconClick"
              @click.prevent="hidePassword = !hidePassword"
              class="material-icons iconEye"
              >{{ icon }}</i
            >
          </div>
          <p v-if="errorMsg" class="errorInput">
            {{ errorMsg }}
          </p>
          <button class="signin" type="submit">Sign In</button>
          <br />
          <p class="signUp">
            <span>Don’t have an account? </span>
            <PersonalRouter
              id="signUp"
              :route="route"
              :buttonText="buttonText"
            />
          </p>
        </form>
      </div>
      <div class="div4">
        <img
          id="travel"
          src="../assets/images/travel.png"
          alt="time to travel"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";

// Route Variables
const route = "/auth/sign-up";
const buttonText = "Sign Up";

// Input Fields
const email = ref("");
const password = ref("");

const icon = ref("visibility_off");

// Error Message
const errorMsg = ref("");

//Show hide password variables
const passwordIconClick = computed(() =>
  hidePassword.value
    ? (icon.value = "visibility_off")
    : (icon.value = "visibility")
);

const passwordFieldType = computed(() =>
  hidePassword.value ? "password" : "text"
);
const hidePassword = ref(true);

// Router to push user once SignedIn to the HomeView
const redirect = useRouter();

// Arrow function to Signin user to supaBase
const signIn = async () => {
  try {
    // calls the user store and send the users info to backend to logIn
    await useUserStore().signIn(email.value, password.value);
    // redirects user to the homeView
    redirect.push({ path: "/" });
  } catch (error) {
    // displays error message
    errorMsg.value = "E-mail & Password required";
    // hides error message
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
};
</script>
test
<style scoped>
.container {
  display: flex;
  flex-direction: row;
  width: 100%;
  align-items: stretch;
  background-image: url(../assets/images/portadafoto.png);
  background-repeat: no-repeat;
  background-size: 65%;
  background-position: bottom right;
}
.left {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding-left: 80px;
}
.pass {
  position: relative;
  width: 100%;
}
.icon {
  width: 80px;
  margin-top: 20px;
}
.iconEye {
  cursor: pointer;
  color: #79351f;
  position: absolute;
  top: 28%;
  right: 8%;
  z-index: 2;
  font-size: 20px;
  opacity: 0.8;
}

#travel {
  width: 160px;
}

.div2 {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
}
.logo {
  width: 350px;
}

.errorInput {
  color: #c42727;
}
.div3 {
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 350px;
  height: 300px;
  overflow: hidden;
  z-index: 1;
}

h2 {
  text-align: center;
  text-transform: uppercase;
  color: #5a3d2b;
}

input {
  display: block;
  height: 50px;
  width: 80%;
  margin: 0 auto;
  border: none;
}

input::placeholder {
  -webkit-transform: translateY(0px);
  transform: translateY(0px);
  -webkit-transition: 0.5s;
  transition: 0.5s;
}

input:hover,
input:focus,
input:active:focus {
  color: #cf4e3e;
  outline: none;
  border-bottom: 1px solid #cf4e3e;
  background-color: #e4e3ce;
  height: 70px;
}

input:focus::placeholder {
  color: #cf4e3e;
  -webkit-transform: translateY(-20px);
  transform: translateY(-20px);
}

.passwordInputType,
.inputEmail {
  position: relative;
  z-index: 1;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  padding-left: 20px;
  font-weight: lighter;
  -webkit-transition: 0.5s;
  transition: 0.5s;
}
.signUp {
  margin-top: 10px;
  text-decoration: none;
  display: inline-block;
  color: #5a3d2b;
  -webkit-transition: 0.5s;
  transition: 0.5s;
}
#signUp {
  color: #5a3d2b;
  font-weight: 600;
}

#signUp:hover {
  text-decoration: underline;
}

button {
  margin-top: 30px;
  align-items: center;
  border: none;
  background: #807d48;
  cursor: pointer;
  border-radius: 3px;
  padding: 10px;
  width: 100px;
  color: #5a3d2d;
  box-shadow: 0 3px 6px 0 rgba(0, 0, 0, 0.2);
  text-align: center;
}

button:hover {
  -webkit-transform: translateY(-3px);
  -ms-transform: translateY(-3px);
  transform: translateY(-3px);
  box-shadow: 0 6px 6px 0 rgba(0, 0, 0, 0.2);
}

@media screen and (max-width: 768px) {
  * {
    margin-top: 0;
    padding: 0;
  }
  .iconEye {
    display: none;
  }
  .icon {
    margin-top: 20px;
    width: 80px;
  }
  .left {
    padding: 0;
    margin-left: 20px;
    justify-content: center;
    align-items: center;
    width: 100%;
  }
  .container {
    margin: 0;
    padding: 0;
    width: 90%;
    align-items: center;
    font-size: 0.8rem;
  }
  .logo {
    width: 250px;
  }
  #travel {
    width: 150px;
    align-items: center;
    margin-top: 0;
    margin-bottom: 0;
  }
  input {
    width: 70%;
  }
  .div3 {
    margin-bottom: 0;
  }
}
@media screen and (max-width: 1024px) and (min-width: 769px) {
  * {
    font-size: 1rem;
  }
  .logo {
    width: 300px;
  }
  .iconEye {
    top: 60px;
  }
  .container {
    width: 100%;
  }
  #travel {
    width: 250px;
    align-items: center;
  }
  input {
    width: 90%;
  }
  .rigth {
    display: none;
  }
}
</style>
