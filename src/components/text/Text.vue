<template>
  <div id="app-text">
      <textarea contenteditable="true" id="text-id" class="app-text content-text" v-model="data">
      </textarea>
  </div>
</template>

<script>
export default {
    name: 'AppText',
    async created() {
        this.input  = async () => {
            if (this.textInput) {
                return this.textInput();
            }

            await this.functions.sleep(0.3);
            return this.input();
        }
        this.$parent.$on('add-value', this.newValue);
    },
    data: () => ({
        data: ''
    }),
    props: {
        functions: {
            type: Object,
            required: true
        }
    },
    methods: {
        getDataLen() {
            return this.data.length;
        },
        async textInput() {
            let afterLen = this.getDataLen();

            this.$emit('text-changed', null);            
            if (!afterLen) return true;
            await this.functions.sleep(0.5);
            let newLen   = this.getDataLen();

            let response = await this.functions.sendText(this.data);
            if (!response) return true;
            let rect = document.getElementById('text-id').getClientRects()[0];
            this.$emit('text-changed', { rect: rect, response: response });
            return true;
        },
        newValue(opt) {
            this.data += opt.text;
            document.getElementById('text-id').focus();
        }
    },
    watch: {
        data: function () {
            this.input();
        }
    }
}
</script>

<style>
    #app-text {
        width: 60%;
        height: 100%;
        margin: 0% auto 0% auto;
    }

    .app-text {
        resize: none;
        border: none;
        overflow: auto;
        color: #636e72;
        background-color: #ffffff;

        margin: 0%;
        width: 100%;
        height: 99.3%;
    }

    .content-text {
        width: 100%;
        margin: 0%;
        padding: 0%;
        padding: 10px;
        font-size: 16px;
        /* height: 18px; */
    }

    .app-text:focus {
        box-shadow: 0 0 0 0;
        border: 0 none;
        outline: 0;
    }
</style>