<template>
  <div id="app-text">
      <div contenteditable="true" id="text-id" class="app-text content-text">
      </div>
  </div>
</template>

<script>
export default {
    name: 'AppText',
    async created() {
        this.doc         = await this.getDocumentDiv();
        this.doc.oninput = () => this.textInput();
    },
    data: () => ({
        doc: null
    }),
    props: {
        functions: {
            type: Object,
            required: true
        }
    },
    methods: {
        async getDocumentDiv() {
            let doc = document.getElementById('text-id');
            if (!doc) {
                await this.functions.sleep(0.3);
                return this.getDocumentDiv()
            };
            return doc;
        },
        getDocVal() {
            return this.doc.innerText;
        },
        getDocLen() {
            return this.doc.innerText.length;
        },
        getRect() {
            const { bottom, height, left, right, top, width, x, y } = this.doc.getClientRects()[0];
            return { bottom, height, left, right, top, width, x, y };
        },
        async textInput(event) {
            let afterLen = this.getDocLen();

            this.$emit('text-changed', null);            
            if (!afterLen) return true;
            await this.functions.sleep(0.5);
            let newLen   = this.getDocLen();

            let response = await this.functions.sendText(this.getDocVal());
            if (!response) return true;
            let rect     = this.getRect();
            this.$emit('text-changed', { rect: rect, response: response });
            return true;
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