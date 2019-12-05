<template>
	<view>
		
		<view class="mglr4 pdtb15">
			<view>
				<textarea value="" placeholder="请填写备注"  placeholder-class="placeholder"/>
			</view>
			<view class="flexEnd fs12 mgt5 color6">0/50个字</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 140rpx;">
			<button class="btn" type="button"  @click="Router.navigateTo({route:{path:'/pages/orderConfirm/orderConfirm'}})">确定</button>
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
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	textarea{background: #fff;border-radius: 10rpx; height: 360rpx;}
</style>

