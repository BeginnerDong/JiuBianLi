<template>
	<view>
		
		<view class="mglr4 pdtb15 flexEnd">
			<view class="myQuanBtn pubColor fs12 center"  @click="Router.navigateTo({route:{path:'/pages/hongbao_myCoupon/hongbao_myCoupon'}})">我的券</view>
		</view>
		<view class="couponlist list mglr4">
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
					{price:'10',title:'10元优惠券',lable:'香烟不可用',infor:'满100元使用'},
					{price:'15',title:'15元优惠券',lable:'香烟不可用',infor:'满200元使用'},
					{price:'20',title:'20元优惠券',lable:'香烟不可用',infor:'满300元使用'}
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
	
	.myQuanBtn{width: 112rpx; height: 40rpx;line-height: 40rpx;background: url(../../static/images/lingquan-img1.png) 0 0 /100% 100%;}
</style>

