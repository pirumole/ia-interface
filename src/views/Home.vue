<template>
  <div id="home">
    <app-text v-bind:functions="functions" v-on:text-changed="showOptions" />
    <app-option v-if="show" 
      v-bind:buttons="buttons" 
      v-bind:functions="functions"
      v-bind:rect="rect"
      v-on:button-click="newValue"
    />
  </div>
</template>

<script>
import App from '../App/App.vue'
import Text from '../components/text/Text';
import Option from '../components/option/Option';

export default {
  name: 'Home',
  components: {
    AppText: Text,
    AppOption: Option
  },
  props: {
    functions: {
      type: Object,
      required: true
    }
  },
  created() {
    this.onResize = async () => {
      if (this.showOptions) {
        return this.showOptions(null);
      }

      await this.functions.sleep(0.5);
      return this.onResize();
    };
    this.$parent.$on('resize', this.onResize);
  },
  data: () => ({
    show    : false,
    buttons : null,
    rect    : null
  }),
  methods: {
    showOptions(data) {
      this.show   = false;
      this.data   = null;
      this.rect   = null;

      if (data == null) return true;

      if (!data.response.result)                return true;
      if (!data.response.result.options)        return true;
      if (!data.response.result.options.length) return true;

      this.show     = true;
      this.buttons  = data.response.result.options;
      this.rect     = data.rect;
    },
    newValue(opt) {
      this.show   = false;
      this.data   = null;

      this.$emit('add-value', opt);
    }
  }
}
</script>

<style>
#home {
  width: 100%;
  height: 96.4%;
  background-color: #dfe6e9;
}
</style>
