<template>
	<view>
		<u-navbar :is-back="true" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>

		<view class="wrap-2">
			<view v-if="flag==0" class="block" :key="index">
				<view class="avatar">

					<u-avatar :src="pic+friendInfo.img+picend" size="200" class="avatar-img">
					</u-avatar>
					<u-form label-position="top" :label-style="{'font-size':'32rpx'}">
						<u-form-item label="手机号">
							<text>{{friendInfo.username}}</text>
						</u-form-item>
						<u-form-item label="用户昵称">
							<text>{{friendInfo.realname}}</text>
						</u-form-item>
						<u-form-item label="性别">
							<text>{{getsex(friendInfo.sex)}}</text>

						</u-form-item>
						<u-form-item label="用户简介">
							<text>{{friendInfo.intro}}</text>

						</u-form-item>
					</u-form>
				</view>

			</view>
		</view>

		<view class="wrap-1" v-if="flag==1">
			<u-form label-position="top" :label-style="{'font-size':'32rpx'}">
				<u-form-item label="群组成员">
				</u-form-item>
			</u-form>
		</view>
		<view class="wrap-2" v-if="flag==1">
			<view class="block" v-for="(item,index) in friendList" :key="index">
				<view class="avatar">

					<u-avatar :src="pic+item.img+picend" size="200" class="avatar-img">
					</u-avatar>
					<view class="text1">
						{{item.friendName}}
					</view>
					<view class="text">
						{{item.friendRealName}}
					</view>
					<view class="text" v-if="index==0" style="color: #fa3534;">
						群主
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
				id: 0,
				flag: 0,
				friendInfo: [],
				friendList: [],

				background: {
					// backgroundColor: '#001f3f',

					// 导航栏背景图
					background: 'url(https://cdn.uviewui.com/uview/swiper/1.jpg) no-repeat',
					// 还可以设置背景图size属性
					backgroundSize: 'cover',

					// 渐变色
					backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))',
				},
			}
		},
		onLoad() {
			uni.$on("sendContactId", (data) => {
				this.id = data.contact;
				this.flag = data.flag; //0代表好友，1代表群组
				if (data.contact != undefined) {
					if (data.flag == 0) {
						uni.request({
							url: 'http://1i682186u9.qicp.vip/user/findById/' + data.contact,
							method: 'GET',
							data: {},
							header: {
								Authorization: helper.tokenHead + '' + helper.token
							},
							success: res => {
								console.log(res.data);
								this.friendInfo = res.data;
							},
							fail: () => {},
							complete: () => {}
						});
					} else {
						uni.request({
							url: 'http://1i682186u9.qicp.vip/group/findGroupUserInfoByGroupId/' + data
								.contact,
							method: 'GET',
							data: {},
							header: {
								Authorization: helper.tokenHead + '' + helper.token
							},
							success: res => {
								console.log(res.data);
								this.friendList = res.data.data;
							},
							fail: () => {},
							complete: () => {}
						});
					}
				}





				console.log(data);
			});
		},
		onUnload() {
			uni.$off("sendContactId");
		},
		methods: {
			getsex(idx) {
				var temp = '';
				if (idx == 0) {
					temp = '女';
				}
				if (idx == 1) {
					temp = '男';
				}
				if (idx == 2) {
					temp = '保密';
				}
				return temp;
			},
			getcolor(idx) {
				var temp = '';
				if (parseFloat(idx) >= 37.3) {
					temp = '#fa3534';
				} else {
					temp = '#19be6b';
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
		margin-left: 24px;
		
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

	.groupInfo {
		font-size: 32rpx
	}


	.avatar {
		display: flex;
		flex-direction: column;
		flex-wrap: wrap;
		margin-top: 10px;

	}

	.text {
		margin-left: 25px;
	}

	.text1 {
		margin-left: 5px;
	}

	.avatar-img {
		padding: 5px;
	}
</style>
