<template>
	<view>
		
		<view class="userHead pdlr4 white">
			<view class="loginBtn flexCenter pdtb20" v-show="is_show" >
				<view class="flexCenter">登录<view class="xian fs16" @click="Router.navigateTo({route:{path:'/pages/login/login'}})"></view>注册</view>
			</view>
			
			<view class="flex userPhoto pdtb20"  v-show="!is_show">
				<view class="flex" @click="Router.navigateTo({route:{path:'/pages/user-infor/user-infor'}})">
					<view class="pic mgr10"><image src="../../static/images/about-img.png" mode=""></image></view>
					<view class="">
						<view class="fs13 mgb5">15964167545</view>
						<view class="lable fs11">新会员</view>
					</view>
				</view>
			</view>
			
			<view class="mydata flex fs13 flexRowBetween pdlr4">
				<view class="item flex flexCenter" @click="Router.navigateTo({route:{path:'/pages/user-myHongbao/user-myHongbao'}})">红包<view class="num">2</view></view>
				<view class="item flex flexCenter" @click="Router.navigateTo({route:{path:'/pages/hongbao_myCoupon/hongbao_myCoupon'}})">优惠券<view class="num">2</view></view>
				<view class="item flex flexCenter">积分<view class="num">2</view></view>
			</view>
			<view class="mglr4 pdtb25 flex">
				<view>余额 <text class="ftw mgl10 mgr20">0.00</text></view>
				<view style="text-decoration: underline" @click="Router.navigateTo({route:{path:'/pages/user-recharge/user-recharge'}})">充值&gt;</view>
			</view>
			
			<view class="flexRowBetween whiteBj orderBtn radius10">
				<view class="flex ll">
					<view class="L-icon">
						<image src="../../static/images/about-img1.png" mode=""></image>
					</view>
					<view class="fs12 color9 mgl10 text avoidOverflow">暂无订单，请选购商品</view>
				</view>
				<view class="fs12 flexEnd color2" @click="Router.navigateTo({route:{path:'/pages/user_order/user_order'}})">全部订单<image class="arrowR" src="../../static/images/about-icon.png" mode=""></image></view>
			</view>
		</view>
		
		<view class="pdlr4">
			<view class="pdb10 fs15">常用功能</view>
			<!-- 分类导航 -->
			<view class="indHome flex pdt10">
				<view class="item"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<image src="../../static/images/about-icon1.png"></image>
					<view class="tit">收货地址</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_myCollect/user_myCollect'}})">
					<image src="../../static/images/about-icon2.png"></image>
					<view class="tit">我的收藏</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/hongbao_Coupon/hongbao_Coupon'}})">
					<image src="../../static/images/about-icon3.png"></image>
					<view class="tit">领券中心</view>
				</view>
				<view class="item">
					<image src="../../static/images/about-icon4.png"></image>
					<view class="tit">分享有礼</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_feedback/user_feedback'}})">
					<image src="../../static/images/about-icon5.png"></image>
					<view class="tit">意见反馈</view>
				</view>
				<view class="item"  @click="Router.navigateTo({route:{path:'/pages/aboutUs/aboutUs'}})">
					<image src="../../static/images/about-icon6.png"></image>
					<view class="tit">关于东轩</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/VIP-Equity/VIP-Equity'}})">
					<image src="../../static/images/about-icon7.png"></image>
					<view class="tit">会员权益</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages//'}})">
					<image src="../../static/images/about-icon8.png"></image>
					<view class="tit">95197</view>
				</view>
			</view>
		</view>
		
		<view class="f5bj pdlr4">
			<view class="titbj flexCenter fs15" style="color: #c9850f">为您推荐</view>
			<!-- 为您推荐 -->
			<view class="proList flex">
				<view class="item" v-for="(item,index) in proList" :key="index" @click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail'}})">
					<view class="pic">
						<image src="../../static/images/home-img10.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
						<view class="flexRowBetween">
							<view class="price">56.00</view>
							<view class="yuanJia">56.00</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/classification/classification'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">分类</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar4-a.png" />
				</view>
				<view class="text this-text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
				proList:[{},{},{},{}]
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
@import "../../assets/style/navbar.css";
page{padding-bottom: 110rpx;}
.userHead{height: 562rpx;background: url(../../static/images/about-img0.png) 0 0 /100% 100%;}
.userHead .loginBtn{line-height: 100rpx;}
.userHead .loginBtn .xian{margin: 0 30rpx;height: 30rpx;width: 1rpx; background: #F5F5F5;}
.userPhoto{color: #F5F5F5;}
.userPhoto .pic{width: 100rpx;height: 100rpx;}
.userPhoto .pic image{width: 100%;height: 100%;border-radius: 50%;}
.userPhoto .lable{background: #7f180f;padding: 0 10rpx;display: inline-block;border-radius: 20rpx;color: #F5F5F5;}

.mydata .item .num{font-size: 30rpx;font-weight: bold;margin-left: 16rpx;}

/* 分类导航 */
.indHome{ flex-wrap:wrap;}
.indHome .item{width: 25%;text-align: center;padding-bottom: 15px; font-size: 24rpx; color: #222;display: flex; flex-direction: column;}
.indHome .item image {width:58rpx;height: 58rpx; margin-bottom: 20rpx;margin: 0 auto 10rpx auto;}

.titbj{height: 40rpx;line-height: 40rpx;padding: 30rpx 0;background: url(../../static/images/details-icon6.png) no-repeat center center/449rpx 20rpx;}

.orderBtn{padding: 40rpx 30rpx;box-shadow: 0 4rpx 10rpx rgba(0,0,0,0.15); margin-bottom: 30rpx;}
.orderBtn .ll{width: 75%;}
.orderBtn .L-icon{width: 70rpx;height: 62rpx;}
.orderBtn .L-icon image{width: 100%;height: 100%; display: block;}
.orderBtn .ll .text{width: 80%;}
</style>

