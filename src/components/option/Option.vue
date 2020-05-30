<template>
  <div id="app-option" v-show="show">
  </div>
</template>

<script>
export default {
    name: 'AppOption',
    props: {
        buttons: {
            type: Array,
            required: true
        },
        functions: {
            type: Object,
            required: true
        },
        rect: {
            type: DOMRect,
            required: true
        }
    },
    data: () => ({
        show: false
    }),
    async created() {
        this.renderButtons();
    },
    methods: {
        async getDocument() {
            let doc = document.getElementById('app-option');
            if (!doc) {
                await this.functions.sleep(0.5);
                return await this.getDocument();
            }
            return doc;
        },
        async removeButtons(button) {
            const value = ' ' + button.innerText + ' ';
            var doc = await this.getDocument();

            while(doc.firstChild) {
                doc.removeChild(doc.firstChild);
            }

            this.$emit('button-click', { text: value });
        },
        async renderButtons() {
            var doc = await this.getDocument();

            doc.style.left = `${this.rect.left - 5}px`  ;
            doc.style.width = `${this.rect.width + 10}px`; 
            this.show = true;
            var _this = this;
            this.buttons.forEach(function (button) {
                let _button = document.createElement('div');
                _button.classList.add('button-text');
                _button.innerText = button;
                _button.onclick   = () => _this.removeButtons(_button);

                doc.appendChild(_button);
            });
        }
    }
}
</script>

<style>
#app-option {
    width: 600px;
    padding: 2px 0px;
    position: fixed;
    background-color: #cccc;
    z-index: 2;
    display: flex;
    bottom: 0px;
    opacity: 0.8;
    overflow-x: auto;
    overflow-y: hidden;
    text-align: center;
    align-items: center;
    justify-content: center;
    scrollbar-width: thin;
    scrollbar-color: #FFFFFF #cccccc;
}

/* *::-webkit-scrollbar-track-piece {
} */

/* *::-webkit-scrollbar-corner {
} */

.button-text {
    margin: auto 5px;
    background-color: #ffffff;
    padding: 5px;
    -webkit-border-radius: 5px;
    cursor: pointer;
}
</style>