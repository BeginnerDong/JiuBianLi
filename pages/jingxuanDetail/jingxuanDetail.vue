<template>
	<view>
		
		<view class="imbox">
			<image class="w" src="../../static/images/img3.png" mode="widthFix"></image>
		</view>
		<view class="flexCenter">
			<view class="q-btn center" @click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail'}})">立即抢购</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{}
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
	@import "../../assets/style/seach.css";
	page{background: #F5F5F5;}
	.q-btn{width: 400rpx;background: #FF2121;color: #fff;border-radius: 40rpx;line-height: 80rpx; margin: 80rpx auto;}
</style>

