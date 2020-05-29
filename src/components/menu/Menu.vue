<template>
  <div id="menu">
		<button class="menu-button">
		    <router-link to="/">Home</router-link>
		</button>
		<button class="menu-button">
		    <router-link to="/about">About</router-link>
		</button>
    	<img class='option' src="../../assets/cardapio.svg">
  </div>
</template>

<script>
export default {
	name: 'Menu',
	created() {
		this.$parent.$on('resize', (event) => this.onResize(event));
	},
	methods: {
		getMenuDocument() {
			return document.getElementById('menu');
		},
		onResize(data) {
			let menu = this.getMenuDocument();
			menu.style.width  = `${data.innerWidth}px` ;
		}
	}
}
</script>

<style>
#menu {
	background-color: #a29bfe;
	height: 25px;
	padding: 5px 0px;
}

.menu-button {
	width: 80px;
	height: 25px;
	margin: 0px 5px;
	background-color: #a29bfe;
	border: none;
	cursor: pointer;
}

.menu-button:hover {
	opacity: 0.4;
	border: 2px #dfe6e9;
}

.menu-button a {
	text-decoration: none;
	color: #ffffff
}

.option {
	position: absolute;
	top: 5px;
	right: 10px;
	width: 25px;
	height: 25px;
	border: none;
	color: #ffffff;
	cursor: pointer;
}

.option:hover {
	opacity: 0.4;
}
</style>