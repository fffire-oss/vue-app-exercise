<template>
	<view>
		<u-navbar :is-back="true" title="OAweb办公系统" title-color="#ffffff" :background="background">
			<view class="navbar-right" slot="right">
				<view class="message-box right-item">
					<u-icon name="chat" size="38" @click="contact(id,flag)"></u-icon>
				</view>
			</view>
		</u-navbar>
		<view class="container">

			<scroll-view class="container-message" id='content' show-scrollbar='true' scroll-with-animation='true'
				scroll-y='true' :scroll-top="scrollTop">
				<view v-for="(item,index) in msg" :key="index" class="container-message-text">
					{{item.to}}:{{item.content}}
				</view>


			</scroll-view>

			<view class="container-textbox">
				<view class="container-input">
					<u-input type="text" v-model="text" placeholder="请输入要发送的消息" />
				</view>
				<view class="container-button">
					<u-button @click="sendMessage" type="info" :ripple="true" ripple-bg-color="#c8c9cc" size='mini'
						shape="circle">发送</u-button>
				</view>
			</view>


		</view>
	</view>
</template>

<script>
	import helper from '../../common/base.js';
	export default {
		data() {
			return {
				//导航
				id: 0,
				flag:0,
				name:'',
				index: 0,
				username: '',
				password: '',
				isShowPassword: true,
				background: {
					// backgroundColor: '#001f3f',

					// 导航栏背景图
					background: 'url(https://cdn.uviewui.com/uview/swiper/1.jpg) no-repeat',
					// 还可以设置背景图size属性
					backgroundSize: 'cover',

					// 渐变色
					backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))',
				},
				data: [{
					a: 1
				}, {
					a: 1
				}, {
					a: 1
				}, {
					a: 1
				}, {
					a: 1
				}, {
					a: 1
				}, {
					a: 1
				}, {
					a: 1
				}, {
					a: 1
				}, {
					a: 1
				}, {
					a: 1
				}],
				//发送
				text: '',
				style: {
					contentHeight: 0,
					msgHeightAll: 0
				},
				msg: [],
				scrollTop: 0,

			}
		},
		watch: {
			msg(newValue, oldValue) {
				// console.log(newValue,oldValue);
				this.scrollToBottom();
			}
		},
		onLoad() {
			this.scrollToBottom();
			//websocket
			uni.connectSocket({
				url: 'ws://1i682186u9.qicp.vip/websocket/' +helper.name,
				
				success: data => {
					console.log(data);
					
				}
			});
			uni.$on("sendChatId", (data) => {
				this.id = data.contact;
				this.name = data.name;
				this.flag = data.flag; //0代表好友，1代表群组




				console.log(data);
			});
			uni.onSocketMessage(function (res) {
			  console.log(res);
			});

		},
		onUnload() {
			uni.$off("sendChatId");
		},

		methods: {
			contact(id,flag) {
				setTimeout(function() {
					uni.$emit("sendContactId", {
						contact: id,
						flag: flag
					})
					
				}, 500);
				uni.redirectTo({
					url: "../contact/contact"
				});
			},
			sendMessage: function() {
				
				this.msg.push({
					to: this.name,
					type:this.flag,
					content: this.text,
					dateTime: new Date().now
				});
				setTimeout(e => {
					uni.sendSocketMessage({
						data: this.msg[length-1],
						
						complete: ()=> {
							this.text = '';
						}
					});
					
				}, 100);
				
				
			},
			scrollToBottom() {
				setTimeout(() => {
					let query = uni.createSelectorQuery();
					query.selectAll(".container-message-text").boundingClientRect(); // 获取所有的消息dom
					query.select("#content").boundingClientRect(); // 获取消息区域dom
					query.exec((res) => {
						this.style.contentHeight = res[1].height.toFixed(2) * 1;
						this.style.msgHeightAll = 0;
						res[0].forEach((rect) => this.style.msgHeightAll = this.style.msgHeightAll + rect
							.height + 12.8)
						if (this.style.msgHeightAll > this.style
							.contentHeight) { // 判断所有内容高度之和是否超出视口高度 超出则设置滚动条的高度
							this.scrollTop = this.style.msgHeightAll - this.style.contentHeight
						}
					})
				})
			},


		}
	}
</script>

<style lang="scss" scoped>
	.container {
		height: 86vh;
		margin-left: 15px;
		margin-right: 15px;
		display: flex;
		justify-content: space-between;
		flex-direction: column;
		// background-color: #F31000;
	}

	.container-message {
		height: 81vh;
		display: flex;
		flex-direction: column-reverse;
		// height: 40px;
		// background-color: #007AFF;
	}

	.container-message-text {
		padding: 20px;
	}

	.container-textbox {
		display: flex;
		flex-direction: row;
		flex-wrap: nowrap;
		// background-color: #18B566;
	}

	.container-button {
		width: 20%;
		margin-left: 20px;
		padding-top: 5px;
	}

	.container-input {
		width: 80%;
	}

	.navbar-right {
		margin-right: 24rpx;
		display: flex;
	}
</style>
