<!-- 聊天会话列表 -->

<template>
	<div id="conversations">
		<div
			:class="{clicked: item.clicked }" 
			v-for="(item, index) in conversations" 
			:key="index"
			class="item"
		>
			<div class="left" @click="clickConversation(index)">
				<img :src="item.avatar">
			</div>
			<div class="right" @click="clickConversation(index)">
				<div class="detail">
					<div class="profile">
						<p class="name">
							<span>{{item.name}}</span>
							<small>{{item.lastestTimeText}}</small>
						</p>	
						<p class="message">{{item.lastestMessage}}</p>	
					</div>					
					<div class="badges">
						<small>{{item.unreadCount}}</small>	
					</div>					
				</div>
			</div>
			<div class="clear"></div>
		</div>
	</div>
</template>

<script>
import { request } from 'http';
export default {
	name: 'Conversation',
	data() {
		return {
			conversations: [{ // 会话列表
				avatar: require('../assets/user.png'),
				name: '阿凡达',
				lastestTimestamp: (new Date()).getTime() - 70 * 1000,
				lastestMessage: '吃饭了吗？',
				lastestTimeText: '',
				unreadCount: 10,
				clicked: false
			}, {
				avatar: require('../assets/user.png'),
				name: '直播姬',
				lastestTimestamp: (new Date()).getTime() - 70 * 1000,
				lastestMessage: '谢谢辣条',
				lastestTimeText: '',
				unreadCount: 100,
				clicked: false
			}],
			timer: null // 定时器
		}
	},
	methods: {
		clickConversation(index) { // 点击会话
			for(let i=0; i<this.conversations.length; i++) {
				this.conversations[i].clicked = false
			}
			this.conversations[index].clicked = true
		},
		updateLastestTimeText() { // 更新时间格式
            let nowTimestamp = (new Date()).getTime()
			for(let i=0; i<this.conversations.length; i++) {
				let item = this.conversations[i]

				let timestampDiff = nowTimestamp - item.lastestTimestamp;
				if (timestampDiff < 0) { // 如果本地时间反而小于变量时间
					item.lastestTimeText = '刚刚';
					continue
				}

				let minute = 1000 * 60;
				let hour = minute * 60;
				let day = hour * 24;
				let halfamonth = day * 15;
				let month = day * 30;

				let monthDiff = timestampDiff / month;
				let weekDiff = timestampDiff / (7 * day);
				let dayDiff = timestampDiff / day;
				let hourDiff = timestampDiff / hour;
				let minDiff = timestampDiff / minute;

				if (monthDiff > 12) { // 超过1年，直接显示年月日
					let date = new Date(item.lastestTimestamp)
					item.lastestTimeText =  date.getFullYear() + '年' + this.fillZero(date.getMonth() + 1) + '月' +this. fillZero(date.getDate()) + '日'
				} else if (monthDiff >= 1) {
					item.lastestTimeText = parseInt(monthDiff) + '月前'
				} else if (weekDiff >= 1) {
					item.lastestTimeText = parseInt(weekDiff) + '周前'
				} else if (dayDiff >= 1) {
					item.lastestTimeText = parseInt(dayDiff) + '天前'
				} else if (hourDiff >= 1) {
					item.lastestTimeText = parseInt(hourDiff) + '小时前'
				} else if (minDiff >= 1) {
					item.lastestTimeText = parseInt(minDiff) + '分钟前'
				} else {
					item.lastestTimeText = '刚刚'
				}
			}

			this.timer = setTimeout(() => { // 每分钟更新一下时间
				this.updateLastestTimeText()
			}, 60000);
		},
		fillZero(value) { // 月日数值前补0
			if (value < 10) {
				return '0' + value
			}
			return value
		}
	},
	mounted() {
		this.updateLastestTimeText()
	},
	beforeDestroy() {
		if(this.timer) { // 销毁时间定时器
			clearInterval(this.timer)
		}
	}
}
</script>

<style lang="less" scoped>
#conversations {
	width: 90%;
	height: auto;
	max-height: 90%;
	margin: 15px 5%;
	background-color: #fff;
	border-radius: 10px;
	overflow: auto;
	box-sizing: border-box;
	padding: 7px 7px 7px 10px;
	box-shadow: 0 0 28px #bbb;

	.item {
		user-select: none;
		cursor: pointer;
		background-color: rgba(255, 255, 255, .5);

		.left {
			float: left;
			position: relative;
			width: 50px;
			margin-right: -50px;
			border-color: transparent;
			transition: border-color ease-out 2s;

			img {
				width: 30px;
				height: 30px;
				display: block;
				margin: 12px 10px;
				border-radius: 5px;
			}
		}
		.right {
			float: left;
			width: 100%;
	
			.detail {
				margin-left: 50px;
				height: 100%;
				display: flex;
				justify-content: space-between;
				padding-bottom: 6px;
				border-bottom: 1px solid #ccc;

				.profile {
					width: calc(100% - 50px);
					display: flex;
					flex-direction: column;
					text-overflow: ellipsis;
					overflow: hidden;
					white-space: nowrap;

					p {
						margin: 0;
					}
					p.name {
						display: flex;
						justify-content: space-between;
						align-items: flex-end;
						margin-top: 13px;

						span {
							font-size: .8rem;
							font-family: '黑体';
							color: #333;
						}
						small {
							font-size: .6rem;
							color: #999;
						}
					}
					p.message {
						font-size: .8rem;
						color: #666;
						margin-top: 5px;
						margin-bottom: 6px;
					}
				}
				.badges {
					width: 30px;
					margin-right: 7px;
					height: 100%;
					display: flex;
					justify-content: center;
					align-self: center;

					small {
						font-size: .7rem;
						text-align: center;
						background-color: #E80505;
						color: #fff;
						display: block;
						padding: 4px;
						border-radius: 10px;
					}
				}
			}
		}
		.clear {
			clear: both;
		}
	}
	.item:hover,
	.item.clicked {
		background-color: #fff;

		.left {
			width: 47px;
			border-left: 3px solid #DE4313;
			border-top-left-radius: 3px;
			border-bottom-left-radius: 3px;

			img {
				margin-left: 7px;
			}
		}
	}
	.item:last-child {
		.right {
			.detail {
				border-bottom-color: transparent;
			}
		}
	}
}
</style>