<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <i>NOTE: Open the browser console to check all the logs.</i>
    <div v-if="isLoggedIn">
      <p>Account is already logged in.</p>
      <button @click="logout">Logout</button>
      <button @click="verify">Verify</button> <br />
      <button @click="getCookies">Get all Cookies</button> <br />
      <strong>Account Info</strong>
      <p>ID: {{ user.id }}</p>
      <p>EMAIL: {{ user.email }}</p>
    </div>
    <div v-else>
      <p>Please login</p>
      <input type="email" placeholder="email" required v-model="email"/> <br/>
      <input type="password" placeholder="password" v-model="password" required/> <br/>
      <button v-if="!isLoggedIn" @click="login">Login</button>
      <br />
    </div>
  </div>
</template>

<script>
import axios from "axios";
const http = axios.create({
  baseURL: "http://localhost:3000",
  withCredentials: true
});

export default {
  name: "HelloWorld",
  data() {
    return {
      email: "admin@email.com",
      password: "P@ssw0rd",
      isLoggedIn: false,
      user: {}
    };
  },
  props: {
    msg: String
  },
  mounted() {
    console.log("Client is creating a new cookie");
    document.cookie = "test1=Hello";
    console.log("Client created a new cookie");
    this.verify();
  },
  methods: {
    login() {
      http
        .post("/login", {
          email: this.email,
          password: this.password
        })
        .then(result => {
          this.isLoggedIn = true;
          console.log(result);
        })
        .catch(err => {
          console.log(err);
        });
    },
    verify() {
      http
        .get("/verify")
        .then(result => {
          if (result.data.user) {
            this.isLoggedIn = true;
          }
          this.user = result.data.user;
          console.log(result);
        })
        .catch(err => {
          console.log(err);
        });
    },
    logout() {
      http
        .post("/logout")
        .then(result => {
          this.isLoggedIn = false;
          this.user = {}
          console.log(result);
        })
        .catch(err => {
          console.log(err);
        });
    },
    getCookies() {
      console.log("All cookies in client: ", document.cookie);
      alert(`Cookies ${document.cookie}`);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
