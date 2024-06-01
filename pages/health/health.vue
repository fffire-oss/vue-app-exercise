<template>
	<view>
		<u-navbar :is-back="true" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>
		<view class="wrap">
			<u-notice-bar mode="horizontal" :list="time"></u-notice-bar>
			<u-form :model="model" :rules="rules" ref="uForm" :errorType="errorType">

				<view class="container">
					<view class="form-username">
						<u-form-item label-width="120" :label-position="labelPosition" label="当前所在地">
							<u-radio-group v-model="value" @change="radioGroupChange">
								<u-radio @change="radioChange" v-for="(item, index) in list" :key="index"
									:name="item.position" :disabled="item.disabled">
									{{item.position}}
								</u-radio>
							</u-radio-group>
						</u-form-item>
					</view>

				</view>


				<u-form-item :leftIconStyle="{color: '#888', fontSize: '32rpx'}" label-width="120"
					:label-position="labelPosition" label="体温" prop="temp">
					<u-input :border="border" placeholder="请输入体温" v-model="model.temp" type="text"></u-input>
				</u-form-item>




			</u-form>

			<u-button @click="submit" type="primary" :ripple="true" ripple-bg-color="#a0cfff" :disabled="disable">提交</u-button>


		</view>
	</view>
</template>

<script>
	import helper from '../../common/base.js';
	var app = getApp();
	export default {
		data() {
			return {
				background: {
					// backgroundColor: '#001f3f',

					// 导航栏背景图
					background: 'url(https://cdn.uviewui.com/uview/swiper/1.jpg) no-repeat',
					// 还可以设置背景图size属性
					backgroundSize: 'cover',

					// 渐变色
					backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
				},
				model: {
					temp: '',
				},
				rules: {
					temp: [{
							required: true,
							message: '请填写当日体温'
						},

						{
							pattern: /^[3-4][0-9]((.[0-9]){0,1})$/gi,
							message: '请输入正确的数字',
							trigger: 'change',
						},
					],
				},
				time:[
					
				],
				disable:false,
				updatetime: '',
				border: false,
				radioCheckWidth: 'auto',
				radioCheckWrap: false,
				labelPosition: 'top',
				codeTips: '',
				errorType: ['message'],
				radioList: [{
					position: '内地',
					disabled: false
				}, {
					position: '香港',
					disabled: false
				}, {
					position: '澳门',
					disabled: false
				}, {
					position: '台湾',
					disabled: false
				}, {
					position: '国外',
					disabled: false
				}],
				radiovalue1: '内地',
				// 基本案列数据
				list: [{
					position: '内地',
					disabled: false
				}, {
					position: '香港',
					disabled: false
				}, {
					position: '澳门',
					disabled: false
				}, {
					position: '台湾',
					disabled: false
				}, {
					position: '国外',
					disabled: false
				}],
				// u-radio-group的v-model绑定的值如果设置为某个radio的name，就会被默认选中
				value: 'orange',
			};
		},
		onUnLoad() {

		},
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},
		onLoad() {
			var time = new Date().toISOString();
			time = time.substr(0, time.length - 1)
			this.updatetime = time;
			this.getTem();
		},
		methods: {
			gettime(timestamp) {
				var year = new Date(timestamp).getFullYear();
				var month = new Date(timestamp).getMonth() + 1;
				var day = new Date(timestamp).getDate();
				var hours = new Date(timestamp).getHours() + 8;
				var minutes = new Date(timestamp).getMinutes();
				if (hours < 10) {
					hours = '0' + hours;
				}
				if (minutes < 10) {
					minutes = '0' + minutes;
				}
				var currentDate = "上次提交时间：" + year + "-" + month + "-" + day + " " + hours + ":" + minutes;
				return currentDate;
			},
			getTem(){
				uni.request({
					url: 'http://1i682186u9.qicp.vip/user/getTem/'+helper.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: helper.tokenHead + '' + helper.token
					},
					success: res => {
						console.log(res);
						if(res.data.updatetime != null){
							// console.log(this.time+ "res.data.updatetime");
							this.time.push(this.gettime(res.data.updatetime));
							var updatetime = new Date(res.data.updatetime).getTime();
							var now = new Date().getTime();
							if(now-updatetime<86400000){
								this.disable = true;
							}
						}
						
					},
					fail: () => {},
					complete: () => {}
				}).bind(this);
			},
			submit() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						
						uni.request({
							url: 'http://1i682186u9.qicp.vip/user/updateTem',
							method: 'POST',
							data: {
								id: helper.id,
								tem: parseFloat(this.model.temp),
								updatetime: this.updatetime
							},
							header: {
								Authorization: helper.tokenHead + '' + helper.token
							},
							success: res => {
								
								if (res.data == '保存成功') {
									uni.showToast({
										title: '提交成功',
										icon: 'success'
									});
								} else {
									uni.showToast({
										title: '提交失败',
										icon: 'none'
									});
								}
							},
							fail: () => {},
							complete: () => {}
						});
					} else {
						console.log('验证失败');
					}
				});
			},
			radioChange(n) {
				console.log('radioChange', n);
			}
		}
	};
</script>

<style scoped lang="scss">
	.wrap {
		padding: 30rpx;
	}

	.agreement {
		display: flex;
		align-items: center;
		margin: 40rpx 0;

		.agreement-text {
			padding-left: 8rpx;
			color: $u-tips-color;
		}
	}

	.container {
		display: flex;
		flex-direction: row;
		justify-content: space-between;

	}

	.avatar {
		display: flex;
		width: 60px;
		margin: 10px;
	}
</style>
