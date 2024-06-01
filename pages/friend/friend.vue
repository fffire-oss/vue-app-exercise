<template>
	<view>
		<u-navbar :is-back="false" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>
		<view class="u-wrap">
			<view class="u-tabs-box">
				<u-tabs-swiper activeColor="#f29100" ref="tabs" :list="list" :current="current" @change="change"
					:is-scroll="false" swiperWidth="750"></u-tabs-swiper>
			</view>
			<view class="">
				<swiper class="swiper-box" :current="swiperCurrent" @transition="transition"
					@animationfinish="animationfinish" style="height: 1100rpx;">
					<swiper-item class="swiper-item">

						<scroll-view scroll-y='true' style="height: 100%;width: 100%;">
							<view class="page-box">
								<view class="order">
									<view class="item" v-for="(item, index) in orderList0" :key="index" @click="chat(item.friendId,0,item.friendName)"
										hover-class="hover">
										<view class="left">
											<u-avatar :src="pic+item.img+picend" size="140" @click="sendMessage">
											</u-avatar>
										</view>
										<view class="content">
											<view class="title u-line-2">{{ item.friendRealName }}</view>
											<view class="type">{{ item.friendName }}</view>
											<view class="type" :style="'color:'+getcolor(item.tem)+ ';'">
												{{ item.tem+'℃'  }}
											</view>
										</view>
									</view>
								</view>

							</view>
						</scroll-view>
					</swiper-item>
					<swiper-item class="swiper-item">
						<scroll-view scroll-y='true' style="height: 100%;width: 100%;" @scrolltolower="reachBottom">
							<view class="page-box">
								<view class="order">
									<view class="item" v-for="(item, index) in orderList1" :key="index" @click="chat(item.id,1,item.name)"
										hover-class="hover">
										<view class="left">
											<u-avatar :src="pic+item.img+picend" size="140"></u-avatar>
										</view>
										<view class="content">
											<view class="title u-line-2">{{ item.name }}</view>
											<view class="type">id:{{ item.id }}</view>
											<view class="type">{{ item.introduction }}</view>
										</view>
									</view>
								</view>

							</view>
						</scroll-view>
					</swiper-item>
					<swiper-item class="swiper-item">

						<scroll-view scroll-y='true' style="height: 100%;width: 100%;">
							<view class="page-box">
								<view class="order">
									<view class="input" style="margin-bottom: 10px;">
										<view class="input1">
											<u-input v-model="value" :type="type" @click="show = true"
												:border="border" />


										</view>

										<view class="input2">
											<u-input :placeholder="placetext" v-model='friendId' :border="border">
											</u-input>
										</view>
									</view>
									<u-select v-model="show" :list="actionSheetList" @confirm="confirm"></u-select>
									<!-- <u-action-sheet :list="actionSheetList" v-model="show" @confirm="confirm">
									</u-action-sheet> -->
									<!-- <button type="default" @click="add">添加</button> -->
									<u-button @click="add" type="info" :ripple="true" ripple-bg-color="#c8c9cc"
										shape="circle">添加</u-button>
								</view>

							</view>

						</scroll-view>
					</swiper-item>
					<swiper-item class="swiper-item">
						<scroll-view scroll-y='true' style="height: 100%;width: 100%;">

							<view class="page-box">
								<view class="order">
									<view style="margin-bottom: 10px;">
										<u-form :model="model" ref="uForm" label-position='top'>
											<u-form-item label="群名称" prop="groupName">
												<u-input placeholder="请输入群聊名称" v-model='model.groupName'
													:border="border" />
											</u-form-item>
											<u-form-item label="简介" prop="groupIntro">
												<u-input placeholder="请输入群简介" v-model='model.groupIntro'
													:border="border" />

											</u-form-item>
											<u-form-item label="邀请好友" prop="friendInvite">
												<u-input type='select' placeholder="邀请好友" v-model='model.friendInvite'
													:border="border" @click='show_p=true' />
												<u-popup v-model="show_p" mode="center" width="500rpx" height="300px"
													border-radius="20" @close="close">
													<view class="pop">
														<text class="pop_text">选择邀请的好友</text>
														<u-checkbox-group>
															<view v-for="(item, index) in orderList0" :key="index">
																<u-checkbox @change="checkboxChange(item.friendId)"
																	v-model="item.checked" v-if="index != 0"
																	:name="item.friendRealName">

																	{{ item.friendRealName }}


																</u-checkbox>
															</view>
														</u-checkbox-group>
													</view>
												</u-popup>

											</u-form-item>
										</u-form>


									</view>



									<u-button @click="createGroup" type="info" :ripple="true" ripple-bg-color="#c8c9cc"
										shape="circle">创建</u-button>
								</view>
							</view>
						</scroll-view>
					</swiper-item>
				</swiper>
			</view>

		</view>
		<u-tabbar v-model="current" :list="tabbar" :mid-button="true"></u-tabbar>
	</view>
</template>

<script>
	import helper from '../../common/base.js';
	export default {
		data() {
			return {
				// avatarid:1,
				pic: '/static/avatar',
				picend: '.png',
				localId: 0,
				current: 0,
				tabbar: '',
				background: {
					// backgroundColor: '#001f3f',

					// 导航栏背景图
					background: 'url(https://cdn.uviewui.com/uview/swiper/1.jpg) no-repeat',
					// 还可以设置背景图size属性
					backgroundSize: 'cover',

					// 渐变色
					backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
				},
				//下拉菜单
				valueDrop: 1,
				options2: [{
						label: '添加好友/群组',
						value: 1,
					},
					{
						label: '创建群组',
						value: 2,
					},
				],
				//添加好友
				friendId: '',

				//创建群组
				model: {
					groupName: '',
					groupIntro: '',
					friendInvite: '',
				},
				rules: {

					groupName: [{
							required: true,
							message: '请输入群组名',
							trigger: 'blur',
						},
						{

							max: 10,
							message: '群组名称不能超过10个字符',
							trigger: ['change', 'blur'],
						},
						{
							pattern: /^[a-z0-9\u4e00-\u9fa5]+$/gi,
							message: '群组名称必须为中文或英文或数字',
							trigger: ['change', 'blur'],
						}
						


					],
					groupIntro: [{
							required: true,
							message: '请输入简介',
							trigger: 'blur',
						},
						{

							max: 40,
							message: '简介字数过多',
							trigger: ['change', 'blur'],
						},
						{
							pattern: /^[\u4e00-\u9fa5]([\u4e00-\u9fa5]|[^%&',;=?$\x22])+$/gi,
							message: '简介只能为中文和标点符号',
							trigger: 'change',
						},


					],
					friendInvite: [{
							required: true,
							message: '请至少邀请一位好友',
							trigger:  'blur',
						},
						
					]

				},

				friendInviteList: [],
				friendList: [{
					img: 1,
					friendName: 'fafa',
					friendRealName: 'fafafa',
					tem: '37.3'
				}, {
					img: 4,
					friendName: 'huahua',
					friendRealName: 'huahuahua',
					tem: '36.3'
				}],
				// friendNameList:[],
				indexList: [],
				show_p: false,

				//下拉选择
				show: false,
				value: '好友',
				placetext: '请输入好友手机号码',
				type: 'select',
				show: false,
				border: true,
				actionSheetList: [{
						value: 1,
						label: '好友'
					},
					{
						value: 2,
						label: '群组'
					},
				],


				//tabswiper
				orderList0: [{
					img: 1,
					friendName: 'fafa',
					friendRealName: 'fafafa',
					tem: '37.3'
				}, {
					img: 4,
					friendName: 'huahua',
					friendRealName: 'huahuahua',
					tem: '36.3'
				}],
				orderList1: [{
					img: 1,
					name: 'fengshengshou',
					intro: 'welcome to fengshengshou',
				}, {
					img: 1,
					name: 'laowangzhe',
					intro: 'welcome to laowangzhe',
				}],

				list: [{
						name: '好友'
					},
					{
						name: '群组'
					},
					{
						name: '添加'
					},
					{
						name: '创建'
					}
				],
				current: 0,
				swiperCurrent: 0,
				tabsHeight: 0,
				dx: 0,
				loadStatus: ['loadmore', 'loadmore', 'loadmore']
			}
		},
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},
		onLoad() {

			//获取好友
			// this.addFriend();
			// //websocket
			// uni.connectSocket({
			// 	url: 'ws://1i682186u9.qicp.vip/websocket/3',
			// 	success: data => {
			// 		console.log(data);
			// 	}
			// });
			//tabswiper
			this.getOrderList(0);
			this.getOrderList(1);

			//tabbar
			this.tabbar = [{
					iconPath: "info-circle",
					selectedIconPath: "info-circle-fill",
					text: '资讯',
					count: 0,
					isDot: false,
					customIcon: false,
					pagePath: '/pages/news/news',
				},
				{
					iconPath: "chat",
					selectedIconPath: "chat-fill",
					text: '联系人',
					customIcon: false,
					pagePath: '/pages/friend/friend'
				},
				{
					iconPath: "https://cdn.uviewui.com/uview/common/min_button.png",
					selectedIconPath: "https://cdn.uviewui.com/uview/common/min_button_select.png",
					text: '发布',
					midButton: true,
					customIcon: false,
					pagePath: '/pages/submit/submit'
				},
				{
					iconPath: "bookmark",
					selectedIconPath: "bookmark-fill",
					text: '任务',
					customIcon: false,
					pagePath: '/pages/mytask/mytask'
				},
				{
					iconPath: "account",
					selectedIconPath: "account-fill",
					text: '我的',
					count: 0,
					isDot: false,
					customIcon: false,
					pagePath: '/pages/my/my'
				}
			];
		},
		onShow() {
			this.getOrderList(0);
			this.getOrderList(1);
		},
		onPullDownRefresh() {
			//获取好友
			// this.addFriend();
			// //websocket
			// uni.connectSocket({
			// 	url: 'ws://1i682186u9.qicp.vip/websocket/3',
			// 	success: data => {
			// 		console.log(data);
			// 	}
			// });
			// //tabswiper
			this.getOrderList(0);
			this.getOrderList(1);
			setTimeout(e=> {
				uni.stopPullDownRefresh();
			}, 2000);
		},
		methods: {
			getcolor(idx) {
				var temp = '';
				if (parseFloat(idx) >= 37.3) {
					temp = '#fa3534';
				} else {
					temp = '#19be6b';
				}
				return temp;
			},
			addFriend() {
				//获取好友
				uni.request({
					url: 'http://1i682186u9.qicp.vip/friend/findFriend/' + helper.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: helper.tokenHead+''+helper.token
					},
					success: res => {
						console.log(res.data);

						this.friendList = res.data;
						this.friendList.splice(0, 1);

					},
					fail: () => {},
					complete: () => {

					}
				});
			},
			close() {
				this.model.friendInvite = '';
				this.friendInviteList = [];
				for (var i = 0; this.orderList0[i] != undefined; i++) {
					if (this.orderList0[i].checked == true) {

						this.model.friendInvite += this.orderList0[i].friendRealName + '; ';
						this.friendInviteList.push(this.orderList0[i].friendName);
					}
				}
			},
			//添加好友
			add() {
				if (this.value == '好友') {
					uni.request({
						url: 'http://1i682186u9.qicp.vip/friend/save',
						method: 'POST',
						data: {
							myId: helper.id,
							friendUsername: this.friendId,
						},
						header: {
							Authorization: helper.tokenHead+''+helper.token
						},
						success: res => {
							console.log(res.data);
							if (res.data.code == 200) {
								uni.showToast({
									title: '添加成功',
									icon: 'success'
								});
							} else {
								uni.showToast({
									title: '添加失败'
								});
							}

						},
						fail: () => {},
						complete: () => {
							this.getOrderList(0);
							this.friendId = '';
						}
					});
				} else {
					uni.request({
						url: 'http://1i682186u9.qicp.vip/group/join',
						method: 'POST',
						data: {
							userid: helper.id,
							groupid: this.friendId,
						},
						header: {
							Authorization: helper.tokenHead+''+helper.token
						},
						success: res => {
							console.log(res.data);
							if (res.data.code == 200) {
								uni.showToast({
									title: '添加成功',
									icon: 'success'
								});
							} else {
								uni.showToast({
									title: '添加失败'
								});
							}
						},
						fail: () => {},
						complete: () => {
							this.getOrderList(1);
							this.friendId = '';
						}
					});
				}
			},
			//创建群组
			checkboxChange: function(id) {
				this.indexList.push(id);
				this.delete(id);


			},
			delete(id) {
				var index = this.indexList.findIndex(item => {
					if (item == id) {
						return true;
					}
				});
				this.indexList.splice(index, 1)
			},
			
			createGroup() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						console.log('验证通过'); 
						uni.request({
							url: 'http://1i682186u9.qicp.vip/group/create',
							method: 'POST',
							data: {
								name: this.model.groupName,
								introduction: this.model.groupIntro,
								ownerid: helper.id,
								members: this.friendInviteList
							},
							header: {
								Authorization: helper.tokenHead+''+helper.token
							},
							success: res => {
								if (res.data.code == 200) {
									uni.showToast({
										title: '创建成功',
										icon: 'success'
									});
									
								} else {
									uni.showToast({
										title: '创建失败'
									});
								}
							},
							fail: () => {},
							complete: () => {
								this.getOrderList(2);
								this.model.groupName = '';
								this.model.groupIntro = '';
								this.model.friendInvite = '';
								this.$refs.uForm.resetFields();
							}
						});
					} else {
						uni.showToast({
							title: '验证失败'
						});
					}
				});

			},
			//下拉选择
			// 点击actionSheet回调
			confirm(e) {
				console.log(e);
				this.value = e[0].label;
				this.placetext = this.value == '好友' ? '请输入好友用户名' : '请输入群组id';
			},
			// actionSheetCallback(index) {
			// 	this.value = this.actionSheetList[index].text;
			// 	this.placetext = this.value == '好友' ? '请输入用户id' : '请输入群组id';
			// },

			//websocket
			sendMessage: function() {
				// uni.sendSocketMessage({
				// 	data: '123'
				// })
			},

			chat(id,flag,name) {
				setTimeout(function() {
					uni.$emit("sendChatId", {
						contact: id,
						flag: flag,
						name:name
					})
				
				}, 500);
				uni.navigateTo({
					url: '../chat/chat',
				});
			},


			//tabswiper
			reachBottom() {
				// 此tab为空数据
				if (this.current != 2) {
					this.loadStatus.splice(this.current, 1, "loading")
					setTimeout(() => {
						this.getOrderList(this.current);
					}, 1200);
				}
			},
			// 页面数据
			getOrderList(idx) {

				if (idx == 0) {
					console.log(helper.token+helper.tokenHead);
					uni.request({
						url: 'http://1i682186u9.qicp.vip/friend/findFriend/' + helper.id,
						method: 'GET',
						data: {},
						header: {
							Authorization: helper.tokenHead+''+helper.token
						},
						success: res => {
							console.log(res.data);
							if (res.data.code == 200) {
								this.orderList0 = res.data.data;

							}

						},
						fail: () => {},
						complete: () => {}
					});
				}
				if (idx == 1) {
					uni.request({
						url: 'http://1i682186u9.qicp.vip/group/findByUserId/' + helper.id,
						method: 'GET',
						data: {},
						header: {
							Authorization: helper.tokenHead+''+helper.token
						},
						success: res => {
							console.log(res.data);
							if (res.data.code == 200) {
								this.orderList1 = res.data.data;

							}
						},
						fail: () => {},
						complete: () => {}
					});
				}

			},
			// tab栏切换
			change(index) {
				this.swiperCurrent = index;
				this.getOrderList(index);
			},
			transition({
				detail: {
					dx
				}
			}) {
				this.$refs.tabs.setDx(dx);
			},
			animationfinish({
				detail: {
					current
				}
			}) {
				this.$refs.tabs.setFinishCurrent(current);
				this.swiperCurrent = current;
				this.current = current;
			}

		}
	}
</script>

<style scoped lang="scss">
	/* #ifndef H5 */
	page {
		height: 100%;
		background-color: #f2f2f2;
	}

	/* #endif */
</style>

<style lang="scss" scoped>
	.order {
		width: 710rpx;
		background-color: #ffffff;
		margin: 20rpx auto;
		border-radius: 20rpx;
		box-sizing: border-box;
		padding: 20rpx;
		font-size: 28rpx;

		.top {
			display: flex;
			justify-content: space-between;

			.left {
				display: flex;
				align-items: center;

				.store {
					margin: 0 10rpx;
					font-size: 32rpx;
					font-weight: bold;
				}
			}

			.right {
				color: $u-type-warning-dark;
			}
		}

		.item {
			display: flex;
			// margin: 20rpx 1 1 ;
			// background-color: #007AFF;

			border-bottom: solid #c8c9cc 1px;
			// border-radius: 10rpx;

			.left {
				margin-left: 20rpx;
				margin-right: 20rpx;
				margin-top: 20rpx;
				margin-bottom: 20rpx;


			}

			.content {
				margin: 20rpx;

				.title {
					font-size: 28rpx;
					line-height: 50rpx;
				}

				.type {
					margin: 10rpx 0;
					font-size: 24rpx;
					color: $u-tips-color;
				}

				.delivery-time {
					color: #e5d001;
					font-size: 24rpx;
				}
			}

		}

		.total {
			margin-top: 20rpx;
			text-align: right;
			font-size: 24rpx;

			.total-price {
				font-size: 32rpx;
			}
		}

		.bottom {
			display: flex;
			margin-top: 40rpx;
			padding: 0 10rpx;
			justify-content: space-between;
			align-items: center;

			.btn {
				line-height: 52rpx;
				width: 160rpx;
				border-radius: 26rpx;
				border: 2rpx solid $u-border-color;
				font-size: 26rpx;
				text-align: center;
				color: $u-type-info-dark;
			}

			.evaluate {
				color: $u-type-warning-dark;
				border-color: $u-type-warning-dark;
			}
		}
	}

	.centre {
		text-align: center;
		margin: 200rpx auto;
		font-size: 32rpx;

		image {
			width: 164rpx;
			height: 164rpx;
			border-radius: 50%;
			margin-bottom: 20rpx;
		}

		.tips {
			font-size: 24rpx;
			color: #999999;
			margin-top: 20rpx;
		}

		.btn {
			margin: 80rpx auto;
			width: 200rpx;
			border-radius: 32rpx;
			line-height: 64rpx;
			color: #ffffff;
			font-size: 26rpx;
			background: linear-gradient(270deg, rgba(249, 116, 90, 1) 0%, rgba(255, 158, 1, 1) 100%);
		}
	}

	.hover {
		background-color: #f4f4f5;
	}

	.wrap {
		display: flex;
		flex-direction: column;
		height: calc(100vh - var(--window-top));
		width: 100%;
	}

	.swiper-box {
		flex: 1;
	}

	.swiper-item {
		height: 100%;
	}

	.input {
		display: flex;
		flex-wrap: nowrap;
	}

	.input1 {
		width: 30%;
	}

	.input2 {
		width: 70%;
	}

	.pop {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		margin: 10px;
		// height: 270px;
	}

	.pop_text {
		margin-bottom: 10px;
	}
</style>
