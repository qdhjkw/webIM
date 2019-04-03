<!-- web IM 页面 / 组件 -->

<template>
	<div id="im">
		<div class="left">
			<div class="conversations">
				<conversations></conversations>
			</div>
		</div>
		<div class="right">
			<div class="messaging">
				<messaging></messaging>
			</div>
		</div>
	</div>
</template>

<script>
import Conversations from '../components/Conversations'
import Messaging from '../components/Messaging'
export default {
	components: {
		Conversations,
		Messaging
	}
}
</script>

<style lang="less" scoped>
#im {
	width: 100%;
	height: 100%;
	margin: 0;
	background-color: #f0f0f0;
	overflow: hidden;

	.left {
		float: left;
		position: relative;
		width: 280px;
		height: 100%;
		margin-right: -280px;

		.conversations {
			width: 100%;
			height: 100%;
			display: block;
		}
	}
	.right {
		float: right;
		width: 100%;
		height: 100%;

		.messaging {
			margin-left: 280px;
			height: 100%;
			display: block;
		}
	}
}

@media screen and (min-width: 1024px) {
// 电脑
}
@media screen and (min-width: 768px) and (max-width: 1024px) {
// 平板
}
@media screen and (max-width: 768px) {
// 手机
}
</style>