<template>
	<view>
		<u-navbar :is-back="false" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>
		<view class="wrap">
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
								<view class="container">
									<view v-for="(item,index) in taskList1" :key="item">
										<u-cell-group>
											<u-cell-item icon="attach" :title="item.tasktext" :key="index"
												@click="click1(item)">
											</u-cell-item>
										</u-cell-group>
									</view>

								</view>
							</view>



						</scroll-view>
					</swiper-item>



					<swiper-item class="swiper-item">
						<scroll-view scroll-y='true' style="height: 100%;width: 100%;">
							<view class="page-box">
								<view class="timeline">
									<u-time-line>
										<u-time-line-item v-for="(item,index) in taskList2" :key="item" nodeTop="2">
											<template v-slot:node>
												<view class="u-node" :style="'background:'+getcolor(item)">
													<u-icon name="clock" color="#ffffff" :size="40"></u-icon>
												</view>
											</template>
											<template v-slot:content>
												<view>
													<view class="u-demo">
														<view class="u-demo-wrap2">
															<view class="u-demo-area">
																<u-line-progress mode="line" :percent="getpercent(item)"
																	:active-color="getcolor(item)" :striped="striped"
																	:stripedActive="stripedActive" height="30">
																</u-line-progress>
																<view class="u-demo-area-text">
																	<view class="u-demo-area-text-time">
																		<u-cell-item
																			:title="gettime(item.task.begintime)+' ~ '+gettime(item.task.endtime)"
																			:key="index" @click="click(item)"
																			style="width:500rpx">
																		</u-cell-item>
																	</view>
																	<view class="u-demo-area-text-task">
																		<u-cell-item :title="item.task.tasktext" value='完成进度'
																			:key="index" @click="click(item)"
																			style="width:500rpx">
																		</u-cell-item>
																	</view>
																</view>

															</view>
														</view>
													</view>
												</view>
											</template>
										</u-time-line-item>
									</u-time-line>
								</view>

							</view>




						</scroll-view>
					</swiper-item>
				</swiper>
			</view>


		</view>
		<u-tabbar v-model="current_t" :list="tabbar" :mid-button="true"></u-tabbar>
	</view>
</template>

<script>
	import helper from '../../common/base.js';
	import currentDate from '../../common/util/currentDate.js';
	export default {
		data() {
			return {
				//swiper
				current: 0,
				list: [{
						name: '我发布的'
					},
					{
						name: '我接受的'
					}
				],
				swiperCurrent: 0,
				tabsHeight: 0,
				dx: 0,
				//
				current_t: 0,
				localId: 0,
				now: '',
				show: false,
				// text: '1',
				// percent: 50,
				mode: 'line',
				activeColor: '#19be6b',
				striped: false,
				stripedActive: false,
				current: 0,
				taskIdList: [],


				taskList1: [],
				taskList2: [{
					priority: 1,
					begintime: '2021-12-10T16:00:00.000+00:00',
					endtime: '2021-12-15T16:00:00.000+00:00',
					tasktext: '吃饭'
				}, {
					priority: 2,
					begintime: '2021-12-10T10:00:00.000+00:00',
					endtime: '2021-12-15T10:00:00.000+00:00',
					tasktext: '吃饭'
				}, {
					priority: 3,
					begintime: '2021-12-10T16:00:00.000+00:00',
					endtime: '2021-12-15T16:00:00.000+00:00',
					tasktext: '吃饭'
				}, {
					priority: 1,
					begintime: '2021-12-09T16:00:00.000+00:00',
					endtime: '2021-12-10T16:00:00.000+00:00',
					tasktext: '吃饭',

				}, ],
				numList: [{
						begintime: '8:00',
						endtime: '8:00',
						tasktext: '吃饭',
						percent: 80.53,
						color: 'red',
						priority: 1
					},
					{
						begintime: '8:00',
						endtime: '8:00',
						tasktext: '睡觉',
						percent: 30,
						color: 'green',
						priority: 1
					},
					{
						begintime: '8:00',
						endtime: '8:00',
						tasktext: '打豆豆',
						percent: 50,
						color: '#f29100',
						priority: 1
					}
				],
				timeList: [{
					year: 0,
					month: 0,
					day: 0,
					hour: 0,
					minute: 0,
					second: 0
				}],
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
				color_index: "#0000ff",
				loadStatus: ['loadmore', 'loadmore']
			}
		},
		onShow() {
			this.getTaskList(0);
			this.getTaskList(1);
		},
		onPullDownRefresh() {
			this.getTaskList(0);
			this.getTaskList(1);
			setTimeout(e => {
				uni.stopPullDownRefresh();
			}, 2000);
		},
		onLoad() {
			this.getTaskList(0);
			this.getTaskList(1);
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
			this.localId = helper.id;

		},

		onPullDownRefresh() {
			this.getTaskList(0);
			this.getTaskList(1);
			setTimeout(e => {
				uni.stopPullDownRefresh();
			}, 2000);
		},

		methods: {
			getTaskList(idx) {
				if (idx == 0) {
					uni.request({
						url: 'http://1i682186u9.qicp.vip/task/findByInitiatorId/' + helper.id,
						method: 'GET',
						data: {},
						header: {
							Authorization: helper.tokenHead + '' + helper.token
						},
						success: res => {
							console.log(res.data);
							// if (res.data.code == 200) {
							this.taskList1 = res.data;
							// }


						},
						fail: () => {},
						complete: () => {

						}
					});
				}
				if (idx == 1) {



					uni.request({
						url: 'http://1i682186u9.qicp.vip/task/findByRecipientId/' + helper.id,
						method: 'GET',
						data: {},
						header: {
							Authorization: helper.tokenHead + '' + helper.token
						},
						success: res => {
							console.log(res.data.data);
							if (res.data.code == 200) {
								this.taskList2 = res.data.data;
							}


						},
						fail: () => {},
						complete: () => {}
					});

					console.log(this.taskList2);

				}


			},

			//查阅任务
			click(id) {
				uni.request({
					url: 'http://1i682186u9.qicp.vip/task/updateUtask',
					method: 'POST',
					data: {
						taskid: id.task.taskid,
						flag: 1,
						recipientid: helper.id

					},
					header: {
						Authorization: helper.tokenHead + '' + helper.token
					},
					success: res => {
						console.log(res.data);
						
					},
					fail: () => {},
					complete: () => {}
				});
				setTimeout(function() {
					uni.$emit("sendTaskDetail", {
						taskId: id.task.taskid,
						tasktext:id.task.tasktext
					});
				}, 500);
				uni.navigateTo({
					url:'../tasksubmit/tasksubmit'
				});
			},
			//任务完成情况
			click1(id) {


				setTimeout(function() {
					uni.$emit("sendTaskId", {
						taskId: id.taskid,
						tasktext:id.tasktext
					});
				}, 500);


				uni.navigateTo({
					url: '/pages/taskdetails/taskdetails',
					success: res => {

					},
					fail: () => {},
					complete: () => {}
				});
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
			},
			//
			gettime(timestamp) {
				var month = new Date(timestamp).getMonth() + 1;
				var day = new Date(timestamp).getDate();
				var hours = new Date(timestamp).getHours();
				var minutes = new Date(timestamp).getMinutes();
				if (hours < 10) {
					hours = '0' + hours;
				}
				if (minutes < 10) {
					minutes = '0' + minutes;
				}
				var currentDate = month + "-" + day + " " + hours + ":" + minutes;
				return currentDate;
			},
			getpercent(tasklist) {
				var begintime = new Date(tasklist.task.begintime).getTime();
				var endtime = new Date(tasklist.task.endtime).getTime();
				var now = new Date().getTime();
				var percent = 0;
				if (now < begintime) {
					percent = 0;
				} else if (now < endtime) {
					percent = (now - begintime) / (endtime - begintime) * 100;
					percent = percent.toFixed(2);
				} else {
					percent = 100;
				}
				if (tasklist.flag == 3) {
					percent = 100;
				}
				return percent;
			},
			getcolor(taskList) {
				if (this.getpercent(taskList) == 100) {
					return '#2979ff';
				} else {
					if (taskList.task.priority == 1) {
						return '#fa3534';
					}
					if (taskList.task.priority == 2) {
						return '#ff9900';
					}
					if (taskList.task.priority == 3) {
						return '#19be6b';
					}
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.u-demo {
		display: flex;
		justify-content: center;
	}

	.text-box {
		display: flex;
		// background-color: #19BE6B;
		height: 40px;
		font-size: 20px;
		padding: 24rpx 24rpx 24rpx 40rpx;
	}

	// .text-box {
	// 	display: flex;
	// 	// background-color: #19BE6B;
	// 	height: 40px;
	// 	font-size: 20px;
	// }


	.u-demo-wrap2 {
		display: flex;
		width: 90%;
		height: 200px;
		flex-direction: column;
		justify-content: space-between;
		padding-right: 30px;
	}

	.u-demo-area-text {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		font-size: 16px;
		margin: 3px;
	}

	.u-demo-area-text-time {
		display: flex;
		// width: 400px;

	}

	.u-demo-area-text-task {
		display: flex;
		// width: 400px;
	}

	.u-demo-popup {
		display: flex;
		flex-direction: row-reverse;
		// background-color: #18B566;
	}


	.u-demo-popup-button {
		margin: 5px;
	}

	.u-config-item {
		display: flex;
		width: 40%;
	}

	.u-progress-dot {
		width: 16rpx;
		height: 16rpx;
		border-radius: 50%;
		background-color: #fb9126;
	}

	.u-progress-info {
		font-size: 28rpx;
		padding-left: 16rpx;
		letter-spacing: 2rpx
	}

	.wrap {
		padding: 24rpx 24rpx 24rpx 40rpx;
	}

	.u-node {
		width: 44rpx;
		height: 44rpx;
		border-radius: 100rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		background: #d0d0d0;

	}

	.u-order-title {
		color: #333333;
		font-weight: bold;
		font-size: 32rpx;
	}

	.u-order-title.unacive {
		color: rgb(150, 150, 150);
	}

	.u-order-desc {
		color: rgb(150, 150, 150);
		font-size: 28rpx;
		margin-bottom: 6rpx;
	}

	.u-order-time {
		color: rgb(200, 200, 200);
		font-size: 26rpx;
	}

	.tel {
		color: $u-type-primary;
	}

	//swiper
	.swiper-box {
		flex: 1;
	}

	.swiper-item {
		height: 100%;
	}

	.timeline {
		margin-left: 20px;
		margin-top: 10px;
	}

	.container {


		margin-top: 10px;
	}
</style>
