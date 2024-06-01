<template>
	<view class="u-demo">
		<view class="u-demo-wrap1">
			<u-steps :list="numList" :current="1" direction="column" mode="number"></u-steps>
		</view>
		<view class="u-demo-wrap2">
			<view class="u-demo-area">
				<u-line-progress mode="line" :percent="percent" :active-color="activeColor"
					:striped="striped" :stripedActive="stripedActive"></u-line-progress>
			</view>
			<view class="u-config-item">
				<u-button :ripple="true" @click="calcChange(0)">减少</u-button>
				<u-button :ripple="true" @click="calcChange(1)">增加</u-button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				percent: 50,
				mode: 'line',
				activeColor: '#19be6b',
				striped: false,
				stripedActive: false,
				current: 0,
				numList: [{
					name: '下单'
				}, {
					name: '出库'
				}, {
					name: '运输'
				}, {
					name: '签收'
				}, ]
			}
		},
		methods: {
			modeChange(index) {
				this.current = index;
				this.mode = index == 0 ? 'line' : 'circle';
			},
			calcChange(index) {
				this.percent = index == 0 ? (this.percent - 10) : (this.percent + 10);
				if (this.percent > 100) this.percent = 100;
				if (this.percent < 0) this.percent = 0;
			},
			styleChange(index) {
				this.activeColor = index == 0 ? this.$u.color['error'] : '#19be6b';
				if (index == 0) {
					this.mode = 'line';
					this.current = 0;
				}
			},
			stripedChange(index) {
				this.striped = index == 0 ? true : false;
				this.stripedActive = this.striped;
				if (index == 0) {
					this.mode = 'line';
					this.current = 0;
				}
			},

		}
	}
</script>

<style lang="scss" scoped>
	.u-demo {
		display: flex;
		justify-content: center;
	}

	.u-demo-wrap1 {
		display: flex;
		width: 20%;
		height: 150px;
		flex-direction: column;
		justify-content: space-between;
		padding-right: 30px;
	}

	.u-demo-wrap2 {
		display: flex;
		width: 60%;
		height: 150px;
		flex-direction: column;
		justify-content: space-between;
		padding-right: 30px;
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
</style>
