<template>
	<view>
		<u-navbar :is-back="false" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>

		<view class="u-flex user-box u-p-l-30 u-p-r-20 u-p-b-30" >
			<view class="u-m-r-10">
				<u-select title='选择头像编号' v-model="show" :list="avatarList" @confirm='changeAvatar'></u-select>
				<u-avatar :src="pic+avatarid+picend" size="140" @click="show = true"></u-avatar>
			</view>
			<view class="u-flex-1">
				<view class="u-font-18 u-p-b-20">{{realName}}</view>
				<view class="u-font-14 u-tips-color">用户名:{{username}}</view>
			</view>
			<view class="u-m-l-10 u-p-10">
				<!-- <u-icon name="scan" color="#969799" size="28"></u-icon> -->
			</view>
			<!-- <view class="u-m-l-10 u-p-10">
				<u-icon name="arrow-right" color="#969799" size="28"></u-icon>
			</view> -->
		</view>


		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="star" title="健康提交"  @click='loading'></u-cell-item>
				
			</u-cell-group>
		</view>

		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="setting" title="设置" hover-class="hover" @click='modifyInfo'></u-cell-item>
			</u-cell-group>
		</view>


		<u-button class="button" @click="click" type="warning" :ripple="true" ripple-bg-color="#fcbd71">退出登录</u-button>


		<u-tabbar v-model="current" :list="tabbar" :mid-button="true"></u-tabbar>
	</view>
</template>

<script>
	import helper from '../../common/base.js';
	//

	export default {

		data() {
			return {
				//头像
				avatarid:1,
				pic: '/static/avatar',
				picend: '.png',
				show: false,
				avatarList: [{
						value: '1',
						label: 1
					},
					{
						value: '2',
						label: 2
					},
					{
						value: '3',
						label: 3
					},
					{
						value: '4',
						label: 4
					},
					{
						value: '5',
						label: 5
					},
					{
						value: '6',
						label: 6
					},
					{
						value: '7',
						label: 7
					},
					{
						value: '8',
						label: 8
					},
					{
						value: '9',
						label: 9
					},
				],
				//用户
				username:'',
				realName:'',
				intro:'',
				password:'',
				sex : 0,
				//导航栏部分
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
				// 随意填充一些模拟数据，以撑开元素，供骨架组件查询和模拟块状数据
				



				//用户部分

			}
		},
		onShow() {
			uni.request({
				url: 'http://1i682186u9.qicp.vip/user/findById/'+helper.id,
				method: 'GET',
				data: {},
				header: {
					Authorization: helper.tokenHead+''+helper.token
				},
				success: res => {
					console.log(res.data);
					this.realName = res.data.realname;
					this.avatarid = res.data.img;
					this.sex = res.data.sex;
					this.password = res.data.password;
					this.intro = res.data.intro;
				},
				fail: () => {},
				complete: () => {}
			});
		},
		onPullDownRefresh() {
			
			uni.request({
				url: 'http://1i682186u9.qicp.vip/user/findById/'+helper.id,
				method: 'GET',
				data: {},
				header: {
					Authorization: helper.tokenHead+''+helper.token
				},
				success: res => {
					console.log(res.data);
					this.realName = res.data.realname;
					this.avatarid = res.data.img;
					this.sex = res.data.sex;
					this.password = res.data.password;
					this.intro = res.data.intro;
				},
				fail: () => {},
				complete: () => {}
			});
			setTimeout(e=> {
				uni.stopPullDownRefresh();
			}, 2000);
		},
		onLoad() {
			this.username = helper.name;
			uni.request({
				url: 'http://1i682186u9.qicp.vip/user/findById/'+helper.id,
				method: 'GET',
				data: {},
				header: {
					Authorization: helper.tokenHead+''+helper.token
				},
				success: res => {
					console.log(res.data);
					this.realName = res.data.realname;
					this.avatarid = res.data.img;
					this.sex = res.data.sex;
					this.password = res.data.password;
					this.intro = res.data.intro;
				},
				fail: () => {},
				complete: () => {}
			});
			console.log(helper.name);
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

		methods: {
			modifyInfo(){
				uni.navigateTo({
					url: '/pages/info/info',
					success: res => {
						
					},
					fail: () => {},
					complete: () => {}
				});
			},
			loading(){
				uni.navigateTo({
					url: '/pages/health/health',
					success: res => {
						
					},
					fail: () => {},
					complete: () => {}
				});
			},
			click() {
				uni.removeStorageSync('username');
				uni.removeStorageSync('id');
				uni.removeStorageSync('token');
				uni.removeStorageSync('tokenHead');
				uni.reLaunch({
					url: '/pages/index/index',
					success: res => {
						helper.id=0;
						uni.showToast({
							title: '退出登录',
							icon: 'success'
						});
					},
					fail: () => {},
					complete: () => {}
				});
			},
			changeAvatar(e) {
				console.log(e[0]);
				this.avatarid = e[0].label;
				uni.request({
					url: 'http://1i682186u9.qicp.vip/user/update',
					method: 'POST',
					
					data: {
						id : helper.id,
						username: this.username,
						password: this.password,
						sex: this.sex,
						intro: this.intro,
						realname: this.realName,
						img: this.avatarid
					},
					header: {
						Authorization: helper.tokenHead+''+helper.token
					},
					success: res => {
						console.log(res.data);
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>


<style lang="scss" scoped>
	page{
		background-color: #ededed;
	}
	
	.user-box{
		background-color: #fff;
		margin-top: 10px;
	}
	.hover {
		background-color: #f4f4f5;
	}
</style>
