<template>
    <div id="app">
		<router-view></router-view>
	</div>
</template>

<style>
html, body, #app {
	width: 100%;
	height: 100%;
	margin: 0;
	padding: 0;
}
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}	
::-webkit-scrollbar { /*滚动条整体样式*/
	width: 5px;
	height: 5px;
	background: rgba(0, 0, 0, 0);
}
::-webkit-scrollbar-thumb { /*滚动条里面小方块样式*/
	border-radius: 5px;
	-webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0);
	background: #aaa;
}
::-webkit-scrollbar-track { /*滚动条里面轨道样式*/
	border-radius: 5px;
	-webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0);
	background: rgba(0, 0, 0, 0);
}
</style>
