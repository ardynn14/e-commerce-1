<template>
  <div class="sign-box rounded shadow">
    <div class="d-flex justify-content-center" style="position: relative;top: -100px;">
      <div class="logo-box d-flex justify-content-center align-items-center shadow m-0">
        <!-- <h1>Dirty Paws</h1> -->
        <img style="width: 90%;" src="../assets/logo.png" alt="Logo" />
      </div>
    </div>
    <div
      class="d-flex justify-content-center align-items-center mt-3"
      style="position: relative;top: -100px;"
    >
      <div
        v-if="signIn"
        style="width:100%; transition: 0.5s all"
        class="d-flex flex-column justify-content-between align-items-center"
      >
        <h1>Sign In</h1>
        <hr style="width: 80%;border: 1px solid grey;" />
        <form
          class="d-flex flex-column justify-content-start align-items-start mt-3"
          style="width: 70%"
          @submit.prevent="login()"
        >
          <label>Email</label>
          <input
            type="email"
            autocomplete="off"
            placeholder="Input email"
            class="form-control"
            v-model="email"
          />
          <label class="mt-2">Password</label>
          <input
            type="password"
            autocomplete="off"
            placeholder="Input password"
            class="form-control"
            v-model="password"
          />
          <div class="d-flex justify-content-center align-item-center mt-2" style="width: 100%;">
            <button class="btn">Login</button>
          </div>
        </form>
        <div>
          <h4>
            New user ? Click
            <a href="#" @click.prevent="goRegister()">here</a> for register
          </h4>
        </div>
      </div>
      <div
        v-if="signUp"
        style="width:100%; transition: 0.5s all"
        class="d-flex flex-column justify-content-between align-items-center"
      >
        <h1>Sign Up</h1>
        <hr style="width: 80%;border: 1px solid grey;" />
        <form
          class="d-flex flex-column justify-content-start align-items-start mt-3"
          style="width: 70%"
          @submit.prevent="register()"
        >
          <label>Username</label>
          <input
            type="text"
            autocomplete="off"
            placeholder="Input username"
            class="form-control"
            v-model="username"
          />
          <label class="mt-2">Email</label>
          <input
            type="email"
            autocomplete="off"
            placeholder="Input email"
            class="form-control"
            v-model="email"
          />
          <label class="mt-2">Password</label>
          <input
            type="password"
            autocomplete="off"
            placeholder="Input password"
            class="form-control"
            v-model="password"
          />
          <div
            class="d-flex justify-content-center align-item-center mt-2 mb-2"
            style="width: 100%;"
          >
            <button class="btn">Register</button>
          </div>
        </form>
        <div>
          <h4>
            Have account ? Click
            <a href="#" @click.prevent="goLogin()">here</a> for login
          </h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";
const url = `http://34.87.3.189`;
export default {
  data() {
    return {
      signUp: false,
      signIn: true,
      email: "",
      password: "",
      username: ""
    };
  },
  methods: {
    goLogin() {
      this.signUp = false;
      this.signIn = true;
    },
    goRegister() {
      this.signUp = true;
      this.signIn = false;
    },
    register() {
      axios({
        url: `${url}/users/register`,
        method: "post",
        data: {
          username: this.username,
          email: this.email,
          password: this.password
        }
      })
        .then(({ data }) => {
          this.username = "";
          this.email = "";
          this.password = "";
          localStorage.setItem("token", data.token);
          localStorage.setItem("username", data.username);
          this.$router.push("/home");
          Swal.fire(`Success`, `Register success`, `success`);
        })
        .catch(err => {
          console.log(err);
          Swal.fire("Errors", `Invalid format input`, `error`);
        });
    },
    login() {
      console.log(this.email);
      console.log(this.password);
      axios({
        url: `${url}/users/login`,
        method: "post",
        data: {
          email: this.email,
          password: this.password
        }
      })
        .then(({ data }) => {
          this.email = "";
          this.password = "";
          localStorage.setItem("token", data.token);
          localStorage.setItem("username", data.username);
          this.$router.push("/home");
        })
        .catch(err => {
          console.log(err);
          Swal.fire("Errors", `Wrong email or password`, `error`);
        });
    }
  }
};
</script>

<style scoped>
.logo-box {
  left: 50%;
  border-radius: 50%;
  background-color: whitesmoke;
  width: 200px;
  height: 200px;
}
.sign-box {
  width: 70vh;
  /* height: 50vh; */
  /* background-color: whitesmoke; */
  background-color: rgba(245, 245, 245, 0.746);
}
input:hover {
  border-bottom: 3px solid #000;
}
input:focus {
  border-bottom: 3px solid #000;
}
button:hover {
  color: grey;
  transition: 0.7s all;
}
button {
  color: black;
  transition: 0.7s all;
  font-size: 24px;
}
</style>
