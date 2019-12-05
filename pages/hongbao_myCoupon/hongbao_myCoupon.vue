<template>
	<view>
		
		<view class="couponlist list mglr4 pdt15">
			<view class="item flexRowBetween" v-for="(item,index) in couponShowData" :key="index">
				<view class="flex ll">
					<view class="mny">{{item.price}}</view>
					<view class="infor fs12">
						<view>{{item.title}}({{item.lable}})</view>
						<view class="flex mgt15"><view class="labBtn">{{item.infor}}</view></view>
					</view>
				</view>
				<view class="rr flexCenter">
					<view class="lq-btn">领取</view>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100rpx;">
			<button class="btn" type="button"  @click="Router.navigateTo({route:{path:'/pages/hongbao_Coupon/hongbao_Coupon'}})">领取更多优惠</button>
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
	
</style>

