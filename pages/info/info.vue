<template>
	<view>
		<u-navbar :is-back="true" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>
		<view class="wrap">
			<u-form :model="model" :rules="rules" ref="uForm" :errorType="errorType">

				<view class="container">
					<view class="form-username">
						<u-form-item :leftIconStyle="{color: '#888', fontSize: '32rpx'}" left-icon="account"
							label-width="120" :label-position="labelPosition" label="昵称" prop="realName">
							<u-input :border="border" placeholder="请输入昵称" v-model="model.realName" type="text">
							</u-input>
						</u-form-item>
					</view>
				</view>


				<u-form-item :leftIconStyle="{color: '#888', fontSize: '32rpx'}" label-width="120"
					:label-position="labelPosition" label="手机号码" prop="username">
					<u-input :border="border" placeholder="请输入手机号" v-model="model.username" type="text"></u-input>
				</u-form-item>
				<u-form-item :label-position="labelPosition" label="性别" prop="sex">
					<u-input :border="border" type="select" :select-open="actionSheetShow" v-model="model.sex"
						placeholder="请选择性别" @click="actionSheetShow = true"></u-input>
				</u-form-item>
				<u-form-item :label-position="labelPosition" label="个人简介" prop="intro">
					<u-input type="textarea" :border="border" placeholder="请填写个人简介" v-model="model.intro" />
				</u-form-item>
				
				<u-form-item :label-position="labelPosition" label="密码" prop="password">
					<u-input :password-icon="true" :border="border" type="password" v-model="model.password"
						placeholder="请输入密码"></u-input>
				</u-form-item>
				<u-form-item :label-position="labelPosition" label="确认密码" label-width="150" prop="rePassword">
					<u-input :border="border" type="password" v-model="model.rePassword" placeholder="请确认密码"></u-input>
				</u-form-item>
			</u-form>
			<!-- 	<img id="kaptcha-img" alt="点击更换" title="点击更换"
				                 οnclick="changeVerifyCode(this)" src="../Kaptcha" /> -->
			<u-button @click="test" type="primary" :ripple="true" ripple-bg-color="#a0cfff">提交</u-button>
			<!-- <u-button @click="test" type="primary" :ripple="true" ripple-bg-color="#a0cfff">提交</u-button> -->
			<u-action-sheet :list="actionSheetList" v-model="actionSheetShow" @click="actionSheetCallback">
			</u-action-sheet>
			<u-select mode="single-column" :list="selectList" v-model="selectShow" @confirm="selectConfirm"></u-select>
			<u-picker mode="region" v-model="pickerShow" @confirm="regionConfirm"></u-picker>
			<u-verification-code seconds="60" ref="uCode" @change="codeChange"></u-verification-code>
		</view>
	</view>
</template>

<script>
	import helper from '../../common/base.js';
	var app = getApp();
	export default {
		data() {
			let that = this;
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
				pic: '/static/avatar',
				picend: '.png',
				model: {
					realName: '',
					username: '',
					sex: '',
					intro: '',
					password: '',
					rePassword: '',
				},
				rules: {

					realName: [{
							required: true,
							message: '请输入姓名',
							trigger: 'blur',
						},
						{
							min: 2,
							max: 5,
							message: '姓名长度在2到5个字符',
							trigger: ['change', 'blur'],
						},
						{
							// 此为同步验证，可以直接返回true或者false，如果是异步验证，稍微不同，见下方说明
							validator: (rule, value, callback) => {
								// 调用uView自带的js验证规则，详见：https://www.uviewui.com/js/test.html
								return this.$u.test.chinese(value);

							},
							message: '姓名必须为中文',
							// 触发器可以同时用blur和change，二者之间用英文逗号隔开
							trigger: ['change', 'blur'],
						},
						// 异步验证，用途：比如用户注册时输入完账号，后端检查账号是否已存在
						// {
						// 	trigger: ['blur'],
						// 	// 异步验证需要通过调用callback()，并且在里面抛出new Error()
						// 	// 抛出的内容为需要提示的信息，和其他方式的message属性的提示一样
						// 	asyncValidator: (rule, value, callback) => {
						// 		this.$u.post('/ebapi/public_api/index').then(res => {
						// 			// 如果验证出错，需要在callback()抛出new Error('错误提示信息')
						// 			if(res.error) {
						// 				callback(new Error('姓名重复'));
						// 			} else {
						// 				// 如果没有错误，也要执行callback()回调
						// 				callback();
						// 			}
						// 		})
						// 	},
						// }
					],
					username: [{
							required: true,
							message: '请输入手机号',
							trigger: 'blur',
						},
						{
							// 此为同步验证，可以直接返回true或者false，如果是异步验证，稍微不同，见下方说明
							validator: (rule, value, callback) => {
								// console.log(value === /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]+\S{5,12}$/);
								// return value === /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]+\S{5,12}$/;
								// var pattern = /^(\d+[a-zA-Z]+|[a-zA-Z]+\d+)([0-9a-zA-Z]*)$/;


								// return value === /^(\d+[a-zA-Z]+|[a-zA-Z]+\d+)([0-9a-zA-Z]*)$/;
								return this.$u.test.mobile(value);

							},
							message: '手机号码不正确',
							// 触发器可以同时用blur和change，二者之间用英文逗号隔开
							trigger: ['change', 'blur'],
						},
						// 异步验证，用途：比如用户注册时输入完账号，后端检查账号是否已存在
						// {
						// 	trigger: ['blur'],
						// 	// 异步验证需要通过调用callback()，并且在里面抛出new Error()
						// 	// 抛出的内容为需要提示的信息，和其他方式的message属性的提示一样
						// 	asyncValidator: (rule, value, callback) => {
						// 		this.$u.post('/ebapi/public_api/index').then(res => {
						// 			// 如果验证出错，需要在callback()抛出new Error('错误提示信息')
						// 			if(res.error) {
						// 				callback(new Error('姓名重复'));
						// 			} else {
						// 				// 如果没有错误，也要执行callback()回调
						// 				callback();
						// 			}
						// 		})
						// 	},
						// }
					],
					sex: [{
						required: true,
						message: '请选择性别',
						trigger: 'change'
					}, ],
					intro: [{
							required: true,
							message: '请填写简介'
						},
						{
							min: 5,
							message: '简介不能少于5个字',
							trigger: 'change',
						},
						// 正则校验示例，此处用正则校验是否中文，此处仅为示例，因为uView有this.$u.test.chinese可以判断是否中文
						{
							pattern: /^[\u4e00-\u9fa5]([\u4e00-\u9fa5]|[^%&',;=?$\x22])+$/gi,
							message: '简介只能为中文和标点符号',
							trigger: 'change',
						},
					],
					password: [{
							required: true,
							message: '请输入密码',
							trigger: ['change', 'blur'],
						},
						{
							// 正则不能含有两边的引号
							pattern: /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]+\S{5,12}$/,
							message: '需同时含有字母和数字，长度在6-12之间',
							trigger: ['change', 'blur'],
						}
					],
					rePassword: [{
							required: true,
							message: '请重新输入密码',
							trigger: ['change', 'blur'],
						},
						{
							validator: (rule, value, callback) => {
								return value === this.model.password;
							},
							message: '两次输入的密码不相等',
							trigger: ['change', 'blur'],
						}
					],
				},
				
				show: false,
				border: false,
				check: false,
				selectStatus: 'close',
				actionSheetList: [{
						text: '男'
					},
					{
						text: '女'
					},
					{
						text: '保密'
					}
				],
				actionSheetShow: false,
				pickerShow: false,
				selectShow: false,
				radioCheckWidth: 'auto',
				radioCheckWrap: false,
				labelPosition: 'top',
				codeTips: '',
				errorType: ['message'],
			};
		},
		onUnLoad() {

		},
		computed: {
			borderCurrent() {
				return this.border ? 0 : 1;
			}
		},
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},
		onLoad() {
			this.getMyInfo();
		},
		methods: {
			getMyInfo(){
				uni.request({
					url: 'http://1i682186u9.qicp.vip/user/findById/'+helper.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: helper.tokenHead+''+helper.token
					},
					success: res => {
						console.log(res.data);
						this.model.intro=res.data.intro;
						this.model.realName=res.data.realname;
						this.model.sex=res.data.sex == 1 ? '男' : (this.model.sex == 0 ? '女' : '保密'),
						this.model.username=res.data.username;
						this.model.img=res.data.img;
					},
					fail: () => {},
					complete: () => {}
				});
			},
			submit() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						if (!this.model.agreement) return this.$u.toast('请勾选协议');
						console.log('验证通过');
					} else {
						console.log('验证失败');
					}
				});
			},
			// 点击actionSheet回调
			actionSheetCallback(index) {
				uni.hideKeyboard();
				this.model.sex = this.actionSheetList[index].text;
			},

			borderChange(index) {
				this.border = !index;
			},
			radioCheckboxChange(index) {
				if (index == 0) {
					this.radioCheckWrap = false;
					this.radioCheckWidth = 'auto';
				} else if (index == 1) {
					this.radioCheckWrap = true;
					this.radioCheckWidth = 'auto';
				} else if (index == 2) {
					this.radioCheckWrap = false;
					this.radioCheckWidth = '50%';
				}
			},
			labelPositionChange(index) {
				this.labelPosition = index == 0 ? 'left' : 'top';
			},
			codeChange(text) {
				this.codeTips = text;
			},

			errorChange(index) {
				if (index == 0) this.errorType = ['message'];
				if (index == 1) this.errorType = ['toast'];
				if (index == 2) this.errorType = ['border-bottom'];
				if (index == 3) this.errorType = ['border'];
			},
			
			test() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						console.log('验证通过');
						uni.request({
							url: 'http://1i682186u9.qicp.vip/user/save',
							method: 'POST',
							data: {
								username: this.model.username,
								password: this.model.password,
								sex: this.model.sex === '男' ? 1 : (this.model.sex === '女' ? 0 : 2),
								intro: this.model.intro,
								realname: this.model.realName,
								img: this.model.avatarid,
								id: helper.id
							},
							header: {
								Authorization: helper.tokenHead+''+helper.token
							},
							success: res => {
								if (res.data == 'success') {
									console.log(res.data);

									uni.navigateBack({
										delta: 1,
										animationType: 'pop-out',
										animationDuration: 200
									});
									uni.showToast({
										title: '注册成功',
										icon: 'success'
									});
								} else {
									uni.showToast({
										title: '验证失败'
									});
								}
							},
							fail: () => {},
							complete: () => {

							}
						});
					} else {
						uni.showToast({
							title: '验证失败'
						});
					}
				});

			},
			changeAvatar(e) {
				// console.log(e);
				this.model.avatarid = e[0].value;
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
