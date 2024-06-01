<template>
	<view>
		<u-navbar :is-back="true" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>

		<view class="wrap">
			

			<image src="/static/register.png" mode='widthFix'></image>
			<image src="/static/register1.png" mode='widthFix'></image>
			<image src="/static/load.png" mode='widthFix'></image>
			<image src="/static/task.png" mode='widthFix'></image>
			<image src="/static/add.png" mode='widthFix'></image>
			
		</view>
		
	</view>
</template>

<script>
	import helper from '../../common/base.js';
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

				
			}
		},
		onLoad() {
			
		},
		methods: {
			

		}
	}
</script>

<style lang="scss" scoped>
	.wrap {
		padding: 40rpx;
	}
</style>
