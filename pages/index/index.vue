<template>
	<view>
		<u-navbar :is-back="false" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>
		<view class="load-main">
			<view class="load-box">
				<view class="load-text">
					登录
				</view>
				<view class="load-input">
					<u-input type="text" placeholder-class="load-place" placeholder="请输入账号" v-model="username"
						border=true style="width:650rpx;" />

				</view>
				<view class="load-input">
					<u-input type="password" placeholder-class="load-place" placeholder="请输入密码" v-model="password"
						border=true style="width:650rpx" />
				</view>
				<view class="load-input">
					<u-input class="vcodeinput" type="text" placeholder-class="load-place" placeholder="请输入验证码"
						v-model="vcode" border=true style="width:650rpx" />
					<image class="vcodeimg" :src="vcodesrc" @click="getVcode" mode="aspectFit"></image>
				</view>
				<view class="load-button">
					<u-button class="load-button-style" @click="checklogin" type="warning" :ripple="true"
						ripple-bg-color="#fcbd71">登录</u-button>
				</view>
				<view class="load-register">
					<text style="color: #fa3534;text-decoration:underline;" @click="click()">注册用户</text>
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
				cookies:'',
				respid: 'success',
				id: 0,
				index: 0,
				username: '',
				password: '',
				vcode: '',
				vcodesrc: '',
				isShowPassword: true,
				background: {
					// backgroundColor: '#001f3f',

					// 导航栏背景图
					background: 'url(https://cdn.uviewui.com/uview/swiper/1.jpg) no-repeat',
					// 还可以设置背景图size属性
					backgroundSize: 'cover',

					// 渐变色
					backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
				}
			}
		},
		onLoad() {
			this.p();
			this.getVcode();
			var username = uni.getStorageSync('username');
			var id = uni.getStorageSync('id');
			var token = uni.getStorageSync('token');
			var tokenHead = uni.getStorageSync('tokenHead');
			if (username != '' && token != '' && tokenHead != '' && id != '') {
				helper.name = username;
				helper.id = id;
				helper.token = token;
				helper.tokenHead = tokenHead;
				uni.switchTab({
					url: '/pages/news/news'
				});
			}
		},
		onUnload() {
			uni.setStorageSync('username', res.data.data.username + '');
			

			console.log(helper.id);

			if (this.index == 1) {
				uni.showToast({
					title: '登陆成功',
					icon: 'success'
				});
			}
		},

		methods: {
			p(){
				var endtime = new Date("2022-01-11T12:15:04.138").getTime();
				var now = new Date("2022-01-12T12:15:04.138").getTime();
				console.log(now-endtime);
			},
			getVcode() {
				uni.request({
					url: 'http://1i682186u9.qicp.vip/captcha',
					method: 'GET',
					responseType: 'arraybuffer',
					data: {},
					
					
					success: res => {
						this.cookies=res.header['Set-Cookie'];
						console.log(res.header['Set-Cookie']);
						const arrayBuffer = new Uint8Array(res.data)
						const base64 = "data:image/png;base64," + uni.arrayBufferToBase64(
							arrayBuffer); //这里需要添加前缀
						this.vcodesrc = base64;

					},
					fail: () => {},
					complete: () => {}
				});
			},
			checklogin() {

				//测试
				// uni.switchTab({
				// 	url: '/pages/news/news',
				// 	success: res => {},
				// 	fail: () => {},
				// 	complete: () => {}
				// });
				uni.request({
					// url: 'http://1i682186u9.qicp.vip/user/findAll',
					url: 'http://1i682186u9.qicp.vip/login',
					method: 'POST',
					data: {
						code: this.vcode,
						username: this.username,
						password: this.password

					},
					header:{
						cookie:this.cookies
					},
					withCredentials: true, 
					success: res => {
						// console.log(this.vcode);
						// console.log(this.username);
						// console.log(this.password);
						helper.id = parseInt(res.data.data.id);
						helper.name = this.username;
						helper.token = res.data.data.token;
						helper.tokenHead = res.data.data.tokenHead;
						if (res.data.code == 200) {
							uni.setStorageSync('id', res.data.data.id);
							uni.setStorageSync('token', res.data.data.token + '');
							uni.setStorageSync('tokenHead', res.data.data.tokenHead + '');
							uni.switchTab({
								url: '/pages/news/news'
							});

							// uni.$emit('sendcustomerId',{cid:this.id});
							this.index = 1;
						} else if (res.data.code == 404) {
							uni.showToast({
								title: '密码错误',
								icon: 'none'
							});
							this.index = 0;
						} else {
							uni.showToast({
								title: '请求失败',
								icon: 'none'
							});
							this.index = 0;
						}
					},
					fail: () => {},
					complete: () => {}
				}).bind(this);
			},
			click() {
				uni.navigateTo({
					url: '/pages/register/register'
				});
				// uni.switchTab({
				// 	url:'../my/my'
				// });
			}
		}
	}
</script>


<style>
	page {
		background-color: ##f3f4f6;
	}
</style>

<style lang="scss" scoped>
	.load-main {
		display: flex;
		justify-content: center;
	}

	.load-box {
		display: flex;
		margin-top: 10px;
		width: 90%;
		height: 250px;
		flex-direction: column;
		justify-content: space-between;
	}

	.load-input {
		display: flex;
		// border: 1px solid #cccccc;

		height: 40px;
		align-items: center;
	}

	.load-inputview {
		width: 75%;
	}

	.vcodeinput {
		width: 80%;
	}

	.vcodeimg {
		width: 88px;
		height: 32px;
	}

	.load-place {
		padding-left: 10px;
		font-size: 15px;
	}

	.load-text {
		line-height: 40px;
		font-size: 25px;
	}

	.load-register {
		display: flex;
		margin: 10px;
		justify-content: flex-end;
	}
</style>
