<template>
	<view>
		<u-navbar :is-back="true" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>
		<view class="wrap">
			<u-form label-position="top" :label-style="{'font-size':'32rpx'}">
				<u-form-item label="任务名">
					<text>{{taskname}}</text>
				</u-form-item>
				<u-form-item label="提交内容">
					<text>{{text}}</text>
				</u-form-item>
				<u-form-item label="提交图片">
					<view v-for="(item,index) in picList" :key="item" class="img">
						<image :src="'http://1i682186u9.qicp.vip/resource/'+item" mode='widthFix'></image>
					</view>

				</u-form-item>
			</u-form>
			<view class="button">
				<view class="button-left">
					<u-button @click="pass(taskId)" size="medium" type="success" :ripple="true" ripple-bg-color="#a0cfff">通过
					</u-button>
				</view>
				<view class="button-right">
					<u-button @click="fail(taskId)" size="medium" type="error" :ripple="true" ripple-bg-color="#a0cfff">不通过
					</u-button>
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
				//tabbar
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
				show: false,

				//text
				taskname: '',
				text: '',
				taskId: 0,
				username: '',
				picList: []
			}
		},
		onLoad() {
			uni.$on("sendId", (data) => {
				this.taskId = data.taskId;
				this.username = data.username;
				this.taskname = data.taskname;
				uni.request({
					url: 'http://1i682186u9.qicp.vip/audit/findSubmitByTaskidAndUsername',
					method: 'POST',
					data: {
						taskId: this.taskId,
						username: this.username
					},
					header: {
						Authorization: helper.tokenHead + '' + helper.token
					},
					success: res => {
						console.log(res);
						this.picList = res.data.data.imgUrlList;
						this.text = res.data.data.submit.text;
					},
					fail: () => {},
					complete: () => {}
				});

				console.log(data);
			});
		},
		
		onUnload() {
			uni.$off('sendId');

		},
		methods: {
			pass(id) {
				uni.request({
					url: 'http://1i682186u9.qicp.vip/task/updateUtask',
					method: 'POST',
					data: {
						taskid: this.taskId,
						flag: 4,
						username: this.username,

					},
					header: {
						Authorization: helper.tokenHead + '' + helper.token
					},
					success: res => {
						console.log(res.data);
						if (res.data.flag == 4) {
							uni.showToast({
								title: '审核成功',
								icon: 'success'
							});
						} else {
							uni.showToast({
								title: '提交失败'
							});
						}
					},
					fail: () => {},
					complete: () => {
						setTimeout(function() {
							uni.$emit("sendTaskId", {
								taskId: id,
							})
							
						}, 500);
						uni.redirectTo({
							url:"../taskdetails/taskdetails"
						});
					}
				}).bind(this);
			},
			fail(id) {
				uni.request({
					url: 'http://1i682186u9.qicp.vip/task/updateUtask',
					method: 'POST',
					data: {
						taskid: this.taskId,
						flag: 3,
						username: this.username,

					},
					header: {
						Authorization: helper.tokenHead + '' + helper.token
					},
					success: res => {
						console.log(res.data);
						if (res.data.flag == 3) {
							uni.showToast({
								title: '审核成功',
								icon: 'success'
							});
						} else {
							uni.showToast({
								title: '审核失败'
							});
						}
					},
					fail: () => {},
					complete: () => {
						setTimeout(function() {
							uni.$emit("sendTaskId", {
								taskId: id,
							})
						}, 500);
						uni.redirectTo({
							url:"../taskdetails/taskdetails"
						});
					}
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.wrap {
		margin: 20px;
	}
	
	.img{
		display: flex;
		flex-wrap: wrap;
	}

	.button {
		display: flex;
		justify-content: center;
	}

	.button-left {
		margin-right: 10px;
	}
</style>
