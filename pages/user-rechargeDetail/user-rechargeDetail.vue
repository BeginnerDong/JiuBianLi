<template>
	<view>
		
		<view class="rechgeMx">
			<view class="item flexRowBetween">
				<view class="tt">
					<view>充值</view>
					<view class="fs12 color6">2019.12.5</view>
				</view>
				<view class="tt price flexCenter">100</view>
				<view class="tt flexEnd"><view class="btn" @click="Router.navigateTo({route:{path:'/pages/orderConfirm-invoice/orderConfirm-invoice'}})">开电子发票</view></view>
			</view>
			<view class="item flexRowBetween">
				<view class="tt">
					<view>充值</view>
					<view class="fs12 color6">2019.12.5</view>
				</view>
				<view class="tt price flexCenter">100</view>
				<view class="tt flexEnd"><view class="btn" @click="Router.navigateTo({route:{path:'/pages/orderConfirm-invoice/orderConfirm-invoice'}})">开电子发票</view></view>
			</view>
			<view class="item flexRowBetween">
				<view class="tt">
					<view>充值</view>
					<view class="fs12 color6">2019.12.5</view>
				</view>
				<view class="tt price flexCenter">100</view>
				<view class="tt flexEnd"><view class="btn finish">发票已开</view></view>
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
				wx_info:{},
				is_show:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
@import "../../assets/style/xieyiAlert.css";

.rechgeMx .item{padding: 20rpx 4%;border-bottom: 1px solid #eee;}
.rechgeMx .item .tt{width: 33.3%;}
.rechgeMx .item .btn{width: 150rpx;text-align: center; line-height: 50rpx;padding: 0 10rpx;border-radius: 8rpx; color: #fff;background: #FF2121; font-size: 24rpx;box-sizing: border-box;}
.rechgeMx .item .btn.finish{background: #ddd;}
</style>

