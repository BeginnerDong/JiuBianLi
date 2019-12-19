<template>
	<view>
		<view class="myExtendTop white">
			<view class="bigNum">533</view>
			<view class="yuan">总积分</view>
		</view>
		
		<view class="myRowBetween pdlr4 fs14" >
			<view class="item flexRowBetween pdtb15" v-for="(item,index) in rewardData" :key="index">
				<view class="ll">
					<view class="avoidOverflow">购买商品</view>
					<view class="color9 mgt5">2019-09-19</view>
				</view>
				<view class="rr red">+200</view>
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
				score:'',
				wx_info:{},
				rewardData:[
					{},{},{}
				]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');

				};
				self.$apis.orderGet(postData, callback);

			},

		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	
	/* 我的积分 */
	.myExtendTop{width: 100%; text-align: center;box-sizing: border-box;padding: 80rpx 4% 80rpx 4%; background: #ff2121;}
	.myExtendTop .money{font-size: 80rpx; padding: 0rpx 4%; line-height:88rpx;}
	.myExtendTop .money:before{content: "￥"; font-size: 50rpx;}
	.myExtendTop .bigNum{font-size: 80rpx; padding: 0rpx 4%; line-height:88rpx;}
	.myExtendTop .yuan{ font-size:28rpx; padding:10rpx 0 30rpx 0;}
	.myExtendTop .txBtn{ width: 200rpx; height: 60rpx;line-height: 60rpx; background: #333; color: #fff; border-radius: 6rpx; margin: 0 auto;font-size: 26rpx;}
	
	.myRowBetween .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}
	
</style>
