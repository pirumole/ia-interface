<template>
  <div id="app-option">
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
        rect: {
            type: Object,
            required: true
        },
        functions: {
            type: Object,
            required: true
        }
    },
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
        async removeButtons() {

        },
        async renderButtons() {
            var doc = await this.getDocument();
            console.log(this.rect);

            doc.style.width   = `${this.rect.width}`; 
            doc.style.height  = `50px`;
            doc.style.bottom  = `5px`;
            doc.style.left    = `${this.rect.left}`;

            this.buttons.forEach(function (button) {
                let _button = document.createElement('div');
                _button.classList.add('button-text');
                _button.innerText = button;
                _button.onclick   = () => this.removeButtons(_button);

                doc.appendChild(_button);
            });
        }
    }
}
</script>

<style>
#app-option {
    width: 100%;
    position: fixed;
    background-color: #cccc;
    z-index: 2;
    bottom: 50px;
    display: flex;
    left: auto;
    right: auto;
    opacity: 0.8;
    justify-items: center;
}

.button-text {
    margin: auto;
    background-color: #ffffff;
    padding: 5px;
    -webkit-border-radius: 5px;
    cursor: pointer;
}
</style>