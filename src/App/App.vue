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
    window.onresize = () => this.onResize();
    this.authorize();
  },
  data: () => ({
    functions: {},
    auth: null,
    url: 'http://10.0.0.107:8181'
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
	position: absolute;
  width: 100%;
  height: 100%;
}
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
} */
</style>