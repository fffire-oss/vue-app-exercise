<template>
	<view>
		<u-navbar :is-back="false" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>

		<view class="wrap">
			
			<view>
				<view class="swiper-box">
					<u-swiper :list="list"></u-swiper>
				</view>
				<image :src="'http://1i682186u9.qicp.vip/resource/'+src" mode='widthFix'></image>
				<u-avatar text="张" fontSize="18" bg-color='#ff9900'></u-avatar>:
				<view class="text-box">
					<view class="text">
						<text >今天也是开心的一天呢</text>
					</view>
					<view class="image">
						<image src="https://lh3.googleusercontent.com/proxy/h4OFoKhHrROcsMuBgJ9wQAM4zDpAQGSeEU5ozFEU5-Rc-WFDJhTWOul54EUQJPObgoLpZJEknDaOi-IUKz2leFkce89JNLIi4Sxc0rTF"></image>
					</view>
					
				</view>
				
			</view>
			<view class="button">
				<u-button class="button" @click="click" type="primary" :ripple="true" ripple-bg-color="#fcbd71">使用说明
				</u-button>
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
				src:'',
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

				//swiper
				list: [{
						image: 'https://cdn.uviewui.com/uview/swiper/1.jpg',
						title: '昨夜星辰昨夜风，画楼西畔桂堂东'
					},
					{
						image: 'https://cdn.uviewui.com/uview/swiper/2.jpg',
						title: '身无彩凤双飞翼，心有灵犀一点通'
					},
					{
						image: 'https://cdn.uviewui.com/uview/swiper/3.jpg',
						title: '谁念西风独自凉，萧萧黄叶闭疏窗，沉思往事立残阳'
					}
				],
			}
		},
		onShow() {
			
		},
		onPullDownRefresh() {
			
			setTimeout(e => {
				uni.stopPullDownRefresh();
			}, 2000);
		},
		onLoad() {
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
			click() {
				uni.navigateTo({
					url: '/pages/illustration/illustration',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}

		}
	}
</script>

<style lang="scss" scoped>
	.wrap {
		padding: 40rpx;
	}

	.button {
		margin-top: 300px;
	}
	.swiper-box{
		margin-bottom: 40px;
		
		
		
	}
	.text-box{
		margin: 10px;
		
	}
	.text{
		margin-bottom: 10px;
	}
</style>
