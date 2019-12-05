<template>
	<view>
		<view class="pubBj center myHead white" style="height: 300rpx;">
			<view class="mny">2562</view>
			<view class="fs12 mgt10">余额(元)</view>
		</view>
		 
		<view class="myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">
					<view>购买商品</view>
					<view class="fs12 color9">2019-11-20</view>
				</view>
				<view class="rr red">-556</view>	
			</view>
			<view class="item flexRowBetween">
				<view class="ll">
					<view>购买商品</view>
					<view class="fs12 color9">2019-11-20</view>
				</view>
				<view class="rr red">-556</view>	
			</view>
			<view class="item flexRowBetween">
				<view class="ll">
					<view>获得红包</view>
					<view class="fs12 color9">2019-11-20</view>
				</view>
				<view class="rr red">+556</view>	
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
.myHead .mny{padding-top: 80rpx;font-size: 70rpx;line-height: 70rpx;}
.myRowBetween .item{padding: 20rpx 4%; border-bottom: 1px solid #eee;}
</style>

