<!-- 聊天消息交互 -->

<template>
	<div id="messaging">
		<div class="main">
			<div v-for="(item, index) in messages" :key="index">
				<div v-if="item.timeText !== ''" class="time">
					<span>{{item.timeText}}</span>
				</div>

				<div class="item" :class="{mine: item.senderId !== converseWith.id}">
					<div class="avatar">
						<img :src="converseWith.avatar">
					</div>
					<div class="content">
						<div class="name">{{converseWith.name}}</div>
						<div v-if="item.type === 'text'" class="message">{{item.content}}</div>

						<div v-if="item.type === 'image'" class="message image" v-viewer="{
							button: false,
							navbar: false,
							title: false,
							toolbar: false
						}">
							<img :src="item.content">
						</div>

						<div v-if="item.type === 'video'" @click="toggleVideoModal(item.content)" class="message video">
							<div class="play">
								<v-icon name="play-circle"></v-icon>
							</div>
							<video :src="item.content"></video>
						</div>

						<div v-if="item.type === 'audio'" class="message audio">
							<v-icon name="volume-up"></v-icon>
						</div>
					</div>
					<div class="clear"></div>
				</div>
			</div>

			<div class="warn">
				<span>阿凡达 不在线</span>
			</div>
		</div>

		<div class="footer">
			<div class="tool">
				<!-- <img src="../assets/image.png"> -->
				<img src="../assets/emoji.png">
				<img src="../assets/file.png">
				<img src="../assets/video.png">
				<img src="../assets/audio.png">
			</div>
			<div class="input">
				<textarea placeholder="聊些什么吧..."></textarea>
			</div>
			<button class="send">
				<v-icon name="plane"></v-icon>
			</button>
			<div class="clear"></div>
		</div>

		<transition name="fade">
			<div @click="toggleVideoModal()" v-if="videoModalConf.isShowVideoModal" class="videoModal">
				<div @click="$event.stopPropagation()" ref="videoModal" class="preview">
					<video-player
						ref="videoPlayer"
						:playsinline="true"
						:options="videoModalConf.playerOptions"
					></video-player>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
import 'video.js/dist/video-js.css'
import { videoPlayer } from 'vue-video-player'

export default {
	name: 'Messaging',
	components: {
		videoPlayer
	},
	data() {
		return {
			/**
			 * @name converseWith 聊天对象
			 * @prop {number} id 用户ID | 群组ID
			 * @prop {string} type single私聊 | group群聊
			 * @prop {string} name 用户昵称 | 群组昵称
			 * @prop {string} avatar 用户头像地址 | 群组头像地址
			 */
			converseWith: {
				id: 2,
				type: 'single',
				name: '阿凡达',
				avatar: require('../assets/user.png')
			},
			/**
			 * @name messages 聊天消息集合
			 * @prop {object} message 聊天消息
			 * @prop {number} message.senderId 消息发送方 用户ID | 群组ID
			 * @prop {number} message.receiverId 消息接收方 用户ID | 群组ID
			 * @prop {string} message.type 消息类型 text文本 | image图片 | video视频 | audio音频
			 * @prop {string} message.content 消息文本或资源地址
			 * @prop {number} message.timestamp 消息发送时间戳
			 * @prop {boolean} message.isShowTime 是否显示时间
			 */
			messages: [{
				senderId: 2,
				receiverId: 1,
				type: 'text',
				content: '你好',
				timestamp: (new Date()).getTime(),
				timeText: ''
			}, {
				senderId: 1,
				receiverId: 2,
				type: 'text',
				content: 'Hello',
				timestamp: (new Date()).getTime(),
				timeText: ''
			}, {
				senderId: 2,
				receiverId: 1,
				type: 'image',
				content: require('../assets/cat.jpg'),
				timestamp: (new Date()).getTime(),
				timeText: ''
			}, {
				senderId: 2,
				receiverId: 1,
				type: 'video',
				content: require('../assets/movie.ogg'),
				timestamp: (new Date()).getTime(),
				timeText: ''
			}, {
				senderId: 2,
				receiverId: 1,
				type: 'audio',
				content: require('../assets/phone.mp3'),
				timestamp: (new Date()).getTime(),
				timeText: ''
			}, {
				senderId: 1,
				receiverId: 2,
				type: 'image',
				content: require('../assets/cat.jpg'),
				timestamp: (new Date()).getTime(),
				timeText: ''
			}, {
				senderId: 1,
				receiverId: 2,
				type: 'video',
				content: require('../assets/movie.ogg'),
				timestamp: (new Date()).getTime() - 300001,
				timeText: ''
			}, {
				senderId: 1,
				receiverId: 2,
				type: 'audio',
				content: require('../assets/phone.mp3'),
				timestamp: (new Date()).getTime(),
				timeText: ''
			}],
			/**
			 * @name videoModalConf 视频播放配置
			 * @prop {boolean} isShowVideoModal 是否播放视频
			 * @prop {object} playerOptions video.js 播放设定 https://github.com/surmon-china/vue-video-player
			 */
			videoModalConf: {
				isShowVideoModal: false,
				playerOptions: {
					width: 600,
					autoplay: true,
					sources: [{
						type: "video/mp4",
						src: "https://cdn.theguardian.tv/webM/2015/07/20/150716YesMen_synd_768k_vp8.webm"
					}]
				}
			}
		}
	},
	methods: {
		/**
		 * @func toggleVideoModal 播放或关闭视频
		 * @param {string} videoSrc 视频地址
		 */
		toggleVideoModal(videoSrc) {
			this.videoModalConf.isShowVideoModal = !this.videoModalConf.isShowVideoModal
			if(videoSrc && this.videoModalConf.isShowVideoModal) {
				this.$nextTick(() => {
					let width = this.$refs.videoModal.getBoundingClientRect().width
					this.videoModalConf.playerOptions.width = width
					this.videoModalConf.playerOptions.sources = [{
						type: 'video/mp4',
						src: videoSrc
					}]
				})
			}
		},
		/**
		 * @func updateMessageTimeText 定时更新时间描述
		 */
		updateMessageTimeText() {
			let nowTimestamp = (new Date()).getTime()
			
			for(let i=0; i<this.messages.length; i++) {
				let prev = i;
				let next = i+1;

				if(prev === 0) {
					this.messages[prev].timeText = this.transTimestamp(this.messages[prev].timestamp)
				}
				if(next < this.messages.length) {
					let prevTimestamp = parseInt(this.messages[prev].timestamp)
					let nextTimestamp = parseInt(this.messages[next].timestamp)
					if((nextTimestamp - prevTimestamp) > 300000) { // 消息间隔超过5分钟
						this.messages[next].timeText = this.transTimestamp(this.messages[next].timestamp)
					}
				}
			}

			this.timer = setTimeout(() => { // 每分钟更新一下时间
				this.updateMessageTimeText()
			}, 60000)
		},
		/**
		 * @private
		 * @func transTimestamp 转化时间戳为直观时间
		 * @param {number} timestamp 时间戳
		 * @return {string}
		 */
		transTimestamp(timestamp) {
			let nowTimestamp = (new Date()).getTime()
			let timestampDiff = nowTimestamp - timestamp;

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
				return date.getFullYear() + '年' + this.fillZero(date.getMonth() + 1) + '月' +this. fillZero(date.getDate()) + '日'
			} else if (monthDiff >= 1) {
				return parseInt(monthDiff) + '月前'
			} else if (weekDiff >= 1) {
				return parseInt(weekDiff) + '周前'
			} else if (dayDiff >= 1) {
				return parseInt(dayDiff) + '天前'
			} else if (hourDiff >= 1) {
				return parseInt(hourDiff) + '小时前'
			} else if (minDiff >= 1) {
				return parseInt(minDiff) + '分钟前'
			} else {
				return '刚刚'
			}
		},
		/**
		 * @private
		 * @func fillZero 在读取到的小于10的月或日数前补0
		 * @param {number} 月或日数
		 * @return {(number | string)}
		 */
		fillZero(value) { // 月日数值前补0
			if (value < 10) {
				return '0' + value
			}
			return value
		}
	},
	mounted() {
		this.updateMessageTimeText()
	},
	beforeDestroy() {
		if(this.timer) { //销毁时间定时器
			clearTimeout(this.timer)
		}
	}
}
</script>

<style lang="less" scoped>
#messaging {
	width: 95%;
	height: 90%;
	margin: 15px 2%;
	overflow: hidden;
	box-sizing: border-box;
	position: relative;
	box-shadow: 0 0 28px #bbb;
	border-radius: 10px;
	background-color: #fff;

	.main { // 聊天内容
		width: 100%;
		height: 70%;
		overflow: auto;
		position: absolute;
		left: 0;
		top: 0;
		box-sizing: border-box;
		padding: 20px;
		
		.time {
			margin-top: 10px;
			padding: 4px 0;
			text-align: center;
			color: #999;
			font-size: .8rem;

			span {
				margin: 0 10px;
			}
			&::before {
				content: ' —— ';
			}
			&::after {
				content: ' —— ';
			}
		}
		.warn {
			margin-top: 10px;

			span {
				display: inline-block;
				border-radius: 6px;
				padding: 6px 22px;
				margin-left: 50%;
				transform: translateX(-50%);
				background-color: #ccc;
				color: #fff;
				text-align: center;
				font-size: .7rem;
			}
		}
		.item {
			margin-top: 10px;

			.avatar {
				float: left;
				width: 60px;
				height: 40px;

				img {
					width: 40px;
					height: 40px;
					display: block;
					margin: 0 10px;
					border-radius: 5px;
				}
			}
			.content {
				float: left;
				margin: 0 10px;
				width: 80%;
				width: calc(100% - 80px);

				.name {
					width: 100%;
					font-size: .8rem;
					font-family: '黑体';
					color: #666;
				}
				.message {
					display: inline-block;
					margin: 10px 0;
					padding: 9px 18px;
					border-top-left-radius: 0;
					border-top-right-radius: 18px;
					border-bottom-right-radius: 18px;
					border-bottom-left-radius: 18px;
					color: #fff;
					background-color: #FE4332;
				}
				.message.image {
					padding: 0;
					max-width: 40%;
					max-height: 50%;
					overflow: hidden;
					background-color: #f0f0f0;
					user-select: none;
					transition: box-shadow ease-in .2s;

					img {
						width: 100%;
						display: block;
						cursor: pointer;
					}
					&:hover {
						box-shadow: 5px 5px 15px #ccc;
					}
				}
				.message.audio {
					padding-left: 25px;
					padding-right: 25px;
					user-select: none;
					cursor: pointer;

					.fa-icon {
						width: auto;
						height: 20px;
						max-width: 100%;
						max-height: 100%;
					}
					&:hover {
						-webkit-filter: brightness(1.5);
					}
				}
				.message.video {
					padding: 0;
					min-width: 40px;
					max-width: 50%;
					min-height: 40px;
					max-height: 50%;
					overflow: hidden;
					background-color: #f0f0f0;
					user-select: none;
					position: relative;
					
					.play {
						position: absolute;
						width: 100%;
						height: 100%;
						background-color: #000;
						text-align: center;
						color: #fff;
						opacity: .5;
						transition: all ease-in .5s;

						.fa-icon {
							position: absolute;
							width: auto;
							height: 40px;
							max-width: 100%;
							max-height: 100%;
							top: 50%;
							left: 50%;
							transform: translate(-50%, -50%);
						}
					}
					video {
						width: 100%;
						display: block;
						cursor: pointer;
					}
					&:hover {
						.play {
							opacity: 1;
							background-color: transparent;
							background-color: rgba(0, 0, 0, .3);
						}
					}
				}
			}
		}
		.item.mine {
			.avatar {
				display: none;
			}
			.content {
				float: right;

				.name {
					display: none;
				}
				.message {
					float: right;
					border-top-left-radius: 18px;
					border-top-right-radius: 18px;
					border-bottom-right-radius: 0;
					border-bottom-left-radius: 18px;
				}
			}
		}
	}
	.footer { // 消息输入
		width: 100%;
		height: 30%;
		height: calc(30% - 1px);
		overflow: auto;
		position: absolute;
		left: 0;
		bottom: 0;
		border-top: 1px solid #e8e8e8;
		
		.tool { // 工具栏
			float: left;
			width: 13%;
			width: calc(13% - 1px);
			height: 100%;
			border-right: 1px solid #e8e8e8;
			overflow: auto;
			text-align: center;

			img {
				cursor: pointer;
				display: block;
				margin: 15px auto 0;
				width: 28px;
				height: 28px;
				transition: all ease-in .7s;
				&:hover {
					transform: scale(1.1, 1.1);
				}
			}
			img:last-child {
				margin-bottom: 15px;
			}
		}
		.input { // 输入框
			float: right;
			width: 87%;
			height: 100%;

			textarea {
				display: block;
				box-sizing: border-box;
				padding: 15px;
				margin: 0;
				width: 100%;
				height: 100%;
				overflow: hidden;
				border: 0;
				outline: none;
				resize: none;
				background-color: #f4f4f4;
				transition: background-color ease-in 3s;
				&:hover,
				&:focus {
					background-color: #fff;
				}
			}
		}
		.send { // 发送按钮
			position: absolute;
			bottom: 30px;
			right: 30px;
			width: 50px;
			height: 50px;
			line-height: 50px;
			background-color: #DE4313;
			border: 0;
			border-radius: 50%;
			color: #fff;
			font-size: 1rem;
			cursor: pointer;
			transition: all .3s ease-in;
			&:hover {
				transform: scale(1.1);
				box-shadow: 6px 3px 8px #999;
				font-size: 1.2rem;
			}
		}
	}
	.videoModal {
		width: 100%;
		height: 100%;
		position: fixed;
		top: 0;
		left: 0;
		background-color: rgba(0, 0, 0, .7);
		z-index: 10;

		.preview {
			width: 55%;
			height: auto;
			background-color: transparent;
			border-radius: 4px;
			position: absolute;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);
		}
	}
	.fade-enter-active, .fade-leave-active {
		transition: opacity .8s;
	}
	.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
		opacity: 0;
	}
	.clear {
		clear: both;
	}
}
</style>