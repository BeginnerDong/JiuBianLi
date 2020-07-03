<template>
	<view>
		
		<view class="couponlist list mglr4">
			<view class="fs13 pdtb15">您有<text class="red" style="padding: 0 6rpx;">{{mainData.length}}</text>张优惠券可用</view>
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index" @click="changeCurr(index)">
				<view class="flex ll">
					<view class="mny">{{item.value}}</view>
					<view class="infor">
						<view class="fs13">{{item.value}}元优惠券</view>
						<view class="flex mgt10 color6 fs12">满{{item.condition}}元可用</view>
					</view>
				</view>
				<view class="rr flexCenter">
					<image class="setIcon" :src="curr==index?'../../static/images/shopping-icon3.png':'../../static/images/shopping-icon2.png'" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100rpx;">
			<button class="btn" type="button"  
			@click="Router.back(1)">确定</button>
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
				curr:-1,
				couponShowData:[
					{price:'5',title:'5元优惠券',lable:'香烟不可用',infor:'满50元使用',},
					{price:'10',title:'10元优惠券',lable:'香烟不可用',infor:'满100元使用'}
				],
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			changeCurr(index){
				const self = this;
				self.curr = index;
				uni.setStorageSync('couponId',self.mainData[self.curr].id);
			},
			
			getMainData() {
				const self = this;
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					use_step: 1,
					type: ['in', [1, 2]],
					invalid_time:['>',now]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
					}
					
					self.$Utils.finishFunc('getMainData');
					
				};
				self.$apis.userCouponGet(postData, callback);
			},
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/couponlist.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	.setIcon{width: 36rpx; height: 36rpx; display: block;}
</style>

