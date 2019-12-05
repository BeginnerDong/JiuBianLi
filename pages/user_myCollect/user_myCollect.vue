<template>
	<view>
		<view class="proList flex pdlr4 mgt15">
			<view class="item" v-for="(item,index) in proList" :key="index" @click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail'}})">
				<view class="pic">
					<image src="../../static/images/home-img10.png" mode=""></image>
				</view>
				<view class="infor">
					<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
					<view class="flexRowBetween">
						<view class="price">56.00</view>
						<view class="yuanJia">56.00</view>
					</view>
				</view>
			</view>
		</view>
		<view class="R-Fixbtn" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
			<view class="num">2</view>
			<view class="icon oh">
				<image src="../../static/images/details-icon9.png" mode=""></image>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				is_show:false,
				wx_info: {},
				proList:[{},{},{}]
			}
		},
		onShow() {
			const self = this;
		},
		methods: {
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/proList.css";
	page{ background: #f5f5f5;}
	.R-Fixbtn .num{line-height: 30rpx; min-width:30rpx;border-radius: 20rpx;background: #FF2121;color: #fff;font-size: 20rpx;position: absolute; top: 0;right: 0rpx;box-sizing: border-box;padding: 0 6rpx;}
	.R-Fixbtn .icon{padding: 26rpx; box-sizing: border-box;background: #fff;border-radius: 50%;box-shadow: 0 2rpx 10rpx rgba(0,0,0,0.15);}
</style>
