<!-- 聊天消息交互 -->

<template>
	<div id="messaging">
		<div class="main">
			<div class="time">
				<span>3天前</span>
			</div>
			<div class="item">
				<div class="avatar">
					<img src="../assets/user.png">
				</div>
				<div class="content">
					<div class="name">阿凡达</div>
					<div class="message">你好</div>
				</div>
				<div class="clear"></div>
			</div>

			<div class="item">
				<div class="avatar">
					<img src="../assets/user.png">
				</div>
				<div class="content">
					<div class="name">阿凡达</div>
					<div class="message image">
						<img src="../assets/cat.jpg">
					</div>
				</div>
				<div class="clear"></div>
			</div>

			<div class="item">
				<div class="avatar">
					<img src="../assets/user.png">
				</div>
				<div class="content">
					<div class="name">阿凡达</div>
					<div class="message video">
						<div class="play">
							<v-icon name="play-circle"></v-icon>
						</div>
						<video src="../assets/movie.ogg"></video>
					</div>
				</div>
				<div class="clear"></div>
			</div>

			<div class="item">
				<div class="avatar">
					<img src="../assets/user.png">
				</div>
				<div class="content">
					<div class="name">阿凡达</div>
					<div class="message voice">
						<v-icon name="volume-up"></v-icon>
					</div>
				</div>
				<div class="clear"></div>
			</div>

			<div class="item mine">
				<div class="avatar">
					<img src="../assets/user.png">
				</div>
				<div class="content">
					<div class="name">阿凡达</div>
					<div class="message video">
						<div class="play">
							<v-icon name="play-circle"></v-icon>
						</div>
						<video src="../assets/movie.ogg"></video>
					</div>
				</div>
				<div class="clear"></div>
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
				<img src="../assets/voice.png">
			</div>
			<div class="input">
				<textarea placeholder="聊些什么吧..."></textarea>
			</div>
			<button class="send">
				<v-icon name="plane"></v-icon>
			</button>
			<div class="clear"></div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Messaging',
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
					max-width: 50%;
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
				.message.voice {
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
					background-color: #FE4332;					
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
				&:hover {
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
	.clear {
		clear: both;
	}
}
</style>