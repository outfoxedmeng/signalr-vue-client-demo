<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <ul>
      <li v-for="(msg, index) in messgesReceived" :key="index">{{ msg }}</li>
    </ul>
    <input v-model="msgToSend" type="text" />
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
      messgesReceived: [],
      connection: Object,
      msgToSend: "",
    };
  },
  mounted() {
    const thisVue = this;
    this.connection = new signalR.HubConnectionBuilder()
      .withUrl("http://localhost:5232/msg-hub")
      .build();

    this.connection.on("ReceiveMsg", (msg) => {
      thisVue.messgesReceived.push(msg);
    });
    this.connection.start();
  },
  methods: {
    sendMsg() {
      this.connection.invoke("BroadcastMsg", this.msgToSend);
      this.msgToSend = "";
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
