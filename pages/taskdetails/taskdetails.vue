<template>
	<view>
		<u-navbar :is-back="true" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>
		<view class="wrap-1">
			<u-form :model="form" ref="uForm" label-position="top" :label-style="{'font-size':'32rpx'}">
				<u-form-item label="任务内容">
					<text>{{tasktext}}</text>
				</u-form-item>
			</u-form>
		</view>
		<view class="wrap-2">


			<view class="block" v-for="(item,index) in friendList" :key="index">
				<view class="avatar" hover-class="avatar-hover" @click="details(item.username,taskid,tasktext)">

					<u-avatar :src="pic+item.img+picend" size="200" class="avatar-img">
					</u-avatar>
					<view class="text">
						{{item.realname}}
					</view>

				</view>
				<view class="info">

					<view class="text">
						<text :style="'color:'+ getColor(item.status)+';'">{{getText(item.status)}}</text>
					</view>
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
				pic: '/static/avatar',
				picend: '.png',
				background: {
					// backgroundColor: '#001f3f',

					// 导航栏背景图
					background: 'url(https://cdn.uviewui.com/uview/swiper/1.jpg) no-repeat',
					// 还可以设置背景图size属性
					backgroundSize: 'cover',

					// 渐变色
					backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
				},
				// recipientIdList: [],
				friendList: [],
				tasktext: '',
				taskid: 0
			}
		},
		onLoad() {
			uni.$on("sendTaskId", (data) => {
				if(data.taskId!=undefined){
					uni.request({
						url: 'http://1i682186u9.qicp.vip/task/findRecipientByTaskId/' + data.taskId,
						method: 'GET',
						data: {},
						header: {
							Authorization: helper.tokenHead + '' + helper.token
						},
						success: res => {
							console.log(1);
							if (res.data.code == 200) {
								this.friendList = res.data.data;
							}
							this.tasktext = data.tasktext;
							this.taskid = data.taskId;
						},
						fail: () => {
					
						},
						complete: () => {}
					}).bind(this);
				}
				

				console.log(data.taskId);
			});
			
			
		},
		

		onUnload() {
			uni.$off('sendTaskId');
			
		},


		methods: {
			details(user, id, tasktext) {
				setTimeout(function() {
					uni.$emit("sendId", {
						taskId: id,
						username: user,
						taskname: tasktext
					}).bind(this);
				}, 500);
				uni.redirectTo({
					url: "../recipientdetails/recipientdetails"
				})
			},
			get(idx) {
				return idx;
			},
			getText(idx) {
				let temp = '';
				if (idx == 0) {
					temp = '未接受';
				}
				if (idx == 1) {
					temp = '已接受';
				}
				if (idx == 2) {
					temp = '已提交';
				}
				if (idx == 3) {
					temp = '审核未通过';
				}
				if (idx == 4) {
					temp = '审核通过';
				}
				if (idx == 5) {
					temp = '超时未完成';
				}
				return temp;
			},


			getColor(idx) {
				let temp = '';
				if (idx == 0) {
					temp = '#82848a';
				}
				if (idx == 1) {
					temp = '#2979ff';
				}
				if (idx == 2) {
					temp = '#8b008b';
				}
				if (idx == 3) {
					temp = '#ff9900';
				}
				if (idx == 4) {
					temp = '#19be6b';
				}
				if (idx == 5) {
					temp = '#fa3534';
				}
				return temp;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.wrap-1 {
		display: flex;
		flex-wrap: wrap;

		padding: 24px;

	}

	.wrap-2 {
		display: flex;
		flex-wrap: wrap;

		padding: 24px;

	}

	.block {
		display: flex;
		flex-direction: column;
		flex-wrap: wrap;
		width: 120px;
		margin: 10px;
		height: 160px;

		// background-color: #007AFF;
	}

	.avatar {
		display: flex;
		flex-direction: column;
		flex-wrap: wrap;
		margin-top: 10px;

	}

	.text {
		margin-left: 30px;
	}

	.info {
		display: flex;
		flex-wrap: nowrap;
	}

	.icon {
		margin: 5px;

	}

	.avatar-hover {
		background-color: #f4f4f5;
	}

	.avatar-img {
		padding: 5px;
	}
</style>
