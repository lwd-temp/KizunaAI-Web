<template>
	<view class="first_view view_center" :style="{'height': winHeight}">
		<view>
			<view class="day view_center">{{birthday}}</view>
			<view class="title view_center">修改你的生日</view>
			<!--多项选择器-->
			<view class="view_center" style="flex-direction: column;">
				<view style="margin: 10rpx 0;">
					<uni-combox label="月份" :candidates="months" placeholder="请选择月份" v-model="month"
						@input="month_change"></uni-combox>
				</view>
				<view style="margin: 10rpx 0;">
					<uni-combox label="日期" :candidates="days" placeholder="请选择日期" v-model="day" @input="day_change">
					</uni-combox>
				</view>
			</view>
			<view class="view_center">
				<button class="buto" @click="yes">设置</button>
			</view>
			<view style="display: flex;justify-content: center;align-items: center;color: #808080;">下次启动时刷新日历生日标签</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				months: [],
				month: "",
				days: [],
				day: "",
				winHeight: "",
				get_month: "",
				get_day: "",
				birthday: ''
			}
		},
		onLoad() {
			let wH = uni.getSystemInfoSync().windowHeight //获取屏幕高度
			this.winHeight = `${wH}px`
			//设置多项选择器内容
			var months = []
			var days = []
			for (var i = 1; i < 13; i++) {
				if (i < 10) {
					i = "0" + i
				} else {
					i = "" + i
				}
				months.push(i)
			}
			for (var i = 1; i < 32; i++) {
				if (i < 10) {
					i = "0" + i
				} else {
					i = "" + i
				}
				days.push(i)
			}
			this.months = months
			this.days = days
			var birthday = uni.getStorageSync("birthday")
			if (birthday) {
				this.birthday = birthday
			}
		},
		methods: {
			month_change(e) {
				this.get_month = e
			},
			day_change(e) {
				this.get_day = e
			},
			yes() {
				var get_month = this.get_month
				var get_day = this.get_day
				if (get_month && get_day) {
					var birthday = get_month + "-" + get_day
					uni.setStorageSync("birthday", birthday)
					uni.showToast({
						icon: "success",
						title: "设置成功",
						duration: 2000
					})
					this.birthday = birthday
				} else {
					uni.showToast({
						icon: "error",
						title: "未设置生日",
						duration: 2000
					})
				}
			},
		}
	}
</script>

<style>
	.day {
		color: #8F939C;
		font-size: 40rpx;
	}

	.first_view {
		width: 100%;
	}

	.title {
		padding: 50rpx 0;
		;
		font-size: 50rpx;
		color: #8F939C;
	}

	.picker_date {
		width: 60%;
		font-size: 40rpx;
		color: #909399;
		border-bottom: 1rpx solid #909399;
	}

	.buto {
		margin: 50rpx 20rpx;
		width: 80%;
	}

	.view_center {
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.now_choose {
		font-size: 40rpx;
		color: #909399;
	}
</style>