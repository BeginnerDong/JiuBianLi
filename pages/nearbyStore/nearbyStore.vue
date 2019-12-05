<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj">
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button"></button>
				<view class="input" style="width: 85%;">
					<input type="text" name="" value="" placeholder="搜索地址/街道/地址" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="Rseach pubColor fs15">搜索</view>
		</view>
		
		<view class="storeList pdlr4">
			<view class="item flexRowBetween" v-for="(item,index) in storeList" :key="index">
				<view class="photo"><image src="../../static/images/stoers-img.png" mode=""></image></view>
				<view class="infor">
					<view class="flexRowBetween">
						<view class="title avoidOverflow">陕西莱安壹江店</view>
						<view class="fs12 color6">1.1km</view>
					</view>
					<view class="adrs fs12 color6">南二环与唐延路东南角华润万家入口</view>
				</view>
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
				is_show:false,
				storeList:[{},{},{},{}]
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
	.storeList .item{align-items: flex-start;padding: 30rpx 0; border-bottom: 1px solid #eee;}
	.storeList .item .photo{width: 180rpx; height: 140rpx;}
	.storeList .item .photo image{width: 100%; height: 100%; display: block;}
	.storeList .item .infor{width: 70%; height: 140rpx;position: relative;}
	.storeList .item .title{width: 75%;}
	.storeList .item .adrs{position: absolute; bottom: 8rpx;left: 0;right: 0;line-height: 32rpx;}
</style>

