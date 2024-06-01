<template>
	<view>
		<u-navbar :is-back="true" title="OAweb办公系统" title-color="#ffffff" :background="background">
		</u-navbar>
		<view class="container">

			功能开发中


		</view>
	</view>
</template>

<script>
	import helper from '../../common/base.js';
	export default {
		data() {
			return {
				//导航
				id:0,
				index: 0,
				username: '',
				password: '',
				isShowPassword: true,
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
		

		methods: {
			


		}
	}
</script>

<style lang="scss" scoped>
	.container {
		height: 86vh;
		margin-left: 15px;
		margin-right: 15px;
		display: flex;
		justify-content: space-between;
		flex-direction: column;
		// background-color: #F31000;
	}

</style>
