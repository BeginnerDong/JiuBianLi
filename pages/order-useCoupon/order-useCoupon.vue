<template>
	<view>
		
		<view class="couponlist list mglr4">
			<view class="fs13 pdtb15">您有<text class="red" style="padding: 0 6rpx;">2</text>张优惠券可用</view>
			<view class="item flexRowBetween" v-for="(item,index) in couponShowData" :key="index" @click="changeCurr(index)">
				<view class="flex ll">
					<view class="mny">{{item.price}}</view>
					<view class="infor">
						<view class="fs13">{{item.title}}({{item.lable}})</view>
						<view class="flex mgt10 color6 fs12">有效期至：2019-12-30</view>
					</view>
				</view>
				<view class="rr flexCenter">
					<image class="setIcon" :src="curr==index?'../../static/images/shopping-icon3.png':'../../static/images/shopping-icon2.png'" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100rpx;">
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
				is_show:false,
				curr:0,
				couponShowData:[
					{price:'5',title:'5元优惠券',lable:'香烟不可用',infor:'满50元使用',},
					{price:'10',title:'10元优惠券',lable:'香烟不可用',infor:'满100元使用'}
				]
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(index){
				const self = this;
				self.curr = index
			},
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
	@import "../../assets/style/couponlist.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	.setIcon{width: 36rpx; height: 36rpx; display: block;}
</style>

