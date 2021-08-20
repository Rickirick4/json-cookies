<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <input type="text" v-model="userName" />
    <button @click="setCookie">set cookie</button>
    <button @click="deleteCookie">Delete cookie</button>
    <button @click="bcryptToken">Crypte Cookie</button>
    <button @click="compareToken">Compare Cookie</button>
    <button @click="tokenJWT">JWT</button>
    <button @click="decodeJWT">Decode JWT</button>
  </div>
</template>

<script>
import bcrypt from "bcryptjs";
import jwt from "jsonwebtoken";
export default {
  name: "App",
  data() {
    return {
      userName: "",
      hashValue: "",
      privateKey: "123143242",
      token:""
    };
  },

  methods: {
    setCookie() {
      this.$cookie.set("token", this.userName, { expires: "1Y" });
    },
    deleteCookie() {
      this.$cookie.delete("token", { domain: "localhost" });
    },
    bcryptToken() {
      const cookieUser = this;
      bcrypt.genSalt(10, function(err, salt) {
        bcrypt.hash(cookieUser.userName, salt, function(err, hash) {
          if (err) {
            console.log(err);
            return;
          }
          console.log(`HASH: ${hash}`);
          //cookieUser.hashValue = this.hash; problem here
        });
      });
    },
    compareToken() {
      const cookieUser = this;
      bcrypt.compare(cookieUser.userName, cookieUser.hash, function(err, res) {
        if (res) {
          console.log("success...");
        }
        if (err) {
          console.log(err);
          return;
        }
      });
    },
    tokenJWT() {
      this.token = jwt.sign({ userName: this.userName }, this.privateKey);
      console.log(this.token);
    },
    decodeJWT() {
      const decoded = jwt.verify(this.token, this.privateKey);
      console.log(decoded);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
