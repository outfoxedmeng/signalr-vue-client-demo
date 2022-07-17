<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <HelloWorld :msg="msg" />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import * as signalR from "@microsoft/signalr";
export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      msg: "",
    };
  },
  mounted() {
    const thisVue = this;
    const connection = new signalR.HubConnectionBuilder()
      .withUrl("http://localhost:5232/msg-hub")
      .build();

    connection.on("ReceiveMsg", (msg) => {
      thisVue.msg = msg;
    });
    connection.start();
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
