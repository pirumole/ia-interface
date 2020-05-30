<template>
  <div id="app">
    <app-menu />
    <router-view v-bind:functions="functions"/>
  </div>
</template>

<script>
import socketIo from 'socket.io-client';
import Menu from '../components/menu/Menu';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    AppMenu: Menu
  },
  created() {
    this.functions  = {
      sendText : this.sendText,
      sleep    : this.sleep
    }
    this.io         = socketIo('http://10.0.0.107:8181/');
    window.onresize = () => this.onResize();
    this.authorize();
  },
  data: () => ({
    functions: {},
    auth: null,
    url : 'http://10.0.0.107:8181'
  }),
  methods: {
    OffSet() {
      let { innerWidth, innerHeight } = window;
      return { innerWidth, innerHeight };
    },
    defaultHeader() {
      let header = {
        'Content-type': 'application/json'
      };

      if (this.auth) header['authorization'] = 'Bearer ' + this.auth;
      return header;
    },
    async request(method, url, data) {
      try {
          let response = await axios({
            method: method.toLocaleUpperCase(),
            url: this.url + url,
            data: data,
            headers: this.defaultHeader()
          });

          if (response.data.status == 'error') throw response.data;
          return response.data;
      } catch (error) {
        return null;
      }
    },
    onResize() {
      this.$emit('resize', this.OffSet());
    },
    async authorize() {
      let response = await this.request('post', '/auth', null)
      if (!response) return true;

      this.auth = response.result.key;
    },
    async renderError() {
    },
    sleep(time) {
      time  = parseFloat(time) || 1;
      time *= 1000;
      
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve(true);
        }, time);
      });
    },
    sendText(text) {
      var _this = this;
      return new Promise((resolve) => {
        this.io.emit('message', { auth: this.auth, text: text }, (err, data) => {
          if (err) {
            _this.renderError(err);
            return resolve(null);
          }

          if (data.status == 'error' && !data.result.autentication) {
            _this.authorize();
            return resolve(null);
          } else
            return resolve(data);
        });
      });
    }
  }
}
</script>
<style>
body {
  position: absolute;
  width: 100%;
  height: 100%;
  padding: 0%;
  margin: 0%;
}

#app {
  width: 100%;
  height: 100%;
}
</style>