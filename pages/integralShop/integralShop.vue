<template>
	<view>
		
		<view class="intg_head pubBj flexRowBetween">
			<view class="item flexColumn">
				<view class="fs24 ftw tit">0</view>
				<view class="icon fs13 flexCenter">积分</view>
			</view>
			<view class="item flexColumn">
				<view class="fs13 tit">积分明细</view>
				<view class="icon flexCenter"><image src="../../static/images/integral-icon.png" mode=""></view>
			</view>
			<view class="item flexColumn">
				<view class="fs13 tit">兑换记录</view>
				<view class="icon flexCenter"><image src="../../static/images/integral-icon1.png" mode=""></image></view>
			</view>
		</view>
				
		<view class="pdlr4 pdt15">
			<view class="fs15 pdb10 ftw">商品</view>
			<view class="proList proList-row">
				<view class="item flexRowBetween" v-for="(item,index) in proList" :key="index" @click="Router.navigateTo({route:{path:'/pages/integralDetail/integralDetail'}})">
					<view class="pic">
						<image src="../../static/images/integral-img.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
						<view class="flexRowBetween B-price fs12 color6">
							<view>单价：50积分</view>
							<view>积分不足</view>
						</view>
					</view>
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
				proList:[{},{},{}]
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
	
	@import "../../assets/style/proList.css";
	page{background: #F5F5F5;}
	.proList-row .item .pic{width: 200rpx; height: 200rpx;}
	.proList-row .item .infor{height: 200rpx;width: 70%;}
	
	.intg_head{padding: 30rpx 4%; color: #F5F5F5;}
	.intg_head .item{width: 33.3%;}
	.intg_head .item .tit{padding-bottom: 20rpx; line-height: 60rpx;}
	.intg_head .item .icon{line-height: 56rpx;}
	.intg_head .item .icon image{width: 56rpx; height: 56rpx;}
</style>

