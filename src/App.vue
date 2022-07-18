<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <ul>
      <li v-for="(msg, index) in messges_received" :key="index">{{ msg }}</li>
    </ul>
    <input v-model="msg_to_send" type="text" />
    <button v-on:click="sendMsg">Send</button>
  </div>
</template>

<script>
import * as signalR from "@microsoft/signalr";
export default {
  name: "App",
  components: {},
  data() {
    return {
      messges_received: [],
      connection: Object,
      msg_to_send: "",
    };
  },
  mounted() {
    const thisVue = this;
    this.connection = new signalR.HubConnectionBuilder()
      .withUrl("http://localhost:5232/msg-hub")
      .build();

    this.connection.on("ReceiveMsg", (msg) => {
      thisVue.messges_received.push(msg);
    });
    this.connection.start();
  },
  methods: {
    sendMsg() {
      this.connection.invoke("BroadcastMsg", this.msg_to_send);
      this.msg_to_send = "";
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
