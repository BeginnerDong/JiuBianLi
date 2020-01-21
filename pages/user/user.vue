<template>
	<view>
		
		<view class="userHead pdlr4 white">
			<view class="loginBtn flexCenter pdtb20" @click="Router.navigateTo({route:{path:'/pages/login/login'}})" v-if="!isLogin" >
				<view class="flexCenter">登录<view class="xian fs16" 
				></view>注册</view>
			</view>
			
			<view class="flex userPhoto pdtb20"  v-if="isLogin">
				<view class="flex" @click="Router.navigateTo({route:{path:'/pages/user-infor/user-infor'}})">
					<view class="pic mgr10">
						<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
					</view>
					<view class="">
						<view class="fs13 mgb5">{{mainData.name}}</view>
						<view class="lable fs11" v-if="mainData.level==0">新会员</view>
						<view class="lable fs11" v-if="mainData.level==1">普通</view>
						<view class="lable fs11" v-if="mainData.level==2">银卡</view>
						<view class="lable fs11" v-if="mainData.level==3">金卡</view>
						<view class="lable fs11" v-if="mainData.level==4">钻石卡</view>
					</view>
				</view>
			</view>
			
			<view class="mydata flex fs13 flexRowBetween pdlr4">
				<view class="item flex flexCenter" 
				@click="Router.navigateTo({route:{path:'/pages/user-myHongbao/user-myHongbao'}})">红包
					<view class="num">{{isLogin&&mainData.bonus?mainData.bonus:'***'}}</view>
				</view>
				<view class="item flex flexCenter" 
				@click="Router.navigateTo({route:{path:'/pages/hongbao_myCoupon/hongbao_myCoupon'}})">
				优惠券<view class="num">{{isLogin&&mainData.coupon?mainData.coupon.length:'***'}}</view>
				</view>
				<view class="item flex flexCenter">积分<view class="num">{{isLogin&&mainData.bonus?mainData.score:'***'}}</view></view>
			</view>
			<view class="mglr4 pdtb25 flex">
				<view>余额 <text class="ftw mgl10 mgr20">{{isLogin&&mainData.bonus?mainData.balance:'***'}}</text></view>
				<view style="text-decoration: underline" 
				@click="Router.navigateTo({route:{path:'/pages/user-recharge/user-recharge'}})">
				充值&gt;
				</view>
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
				<button class="item" open-type="share">
					<image src="../../static/images/about-icon4.png"></image>
					<view class="tit">分享有礼</view>
				</button>
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
				<view class="item" @click="callPhone">
					<image src="../../static/images/about-icon8.png"></image>
					<view class="tit">客服电话</view>
				</view>
			</view>
		</view>
		
		<view class="f5bj pdlr4">
			<view class="titbj flexCenter fs15" style="color: #c9850f">为您推荐</view>
			<!-- 为您推荐 -->
			<view class="proList flex">
				<view class="item" v-for="(item,index) in productData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">
					<view class="pic">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">{{item.title}}</view>
						<view class="flexRowBetween">
							<view class="price">{{item.price}}</view>
							<view class="yuanJia">{{item.o_price}}</view>
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
				is_show:true,
				proList:[{},{},{},{}],
				isLogin:false,
				mainData:{},
				productData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getLabelData','getProductData'], self);
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('user_token')){
				self.isLogin = true
				self.$Utils.loadAll(['getMainData'], self);
			}
		},
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if(!uni.getStorageSync('user_info')){
				uni.showModal({
				    title: '提示',
				    content: '您未登录，登录后再进行此操作',
					showCancel:false,
				    success: function (res) {
				        if (res.confirm) {
				            
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});	
				return
			};
			if (ops.from === 'button') {
				return {
					title: '酒便利',
					path: '/pages/index/index?user_no='+uni.getStorageSync('user_info').user_no, //点击分享的图片进到哪一个页面
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
				console.log(ops.target)
			}else{
				
			}
			
		},
		
		methods: {
			
			
			
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					coupon:{
						tableName:'UserCoupon',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				
				postData.searchItem = {
					thirdapp_id:2,
					title:'客服电话'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data[0];
					}
					console.log('self.labelData', self.labelData)
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			callPhone(phone){
				const self = this;
				uni.makePhoneCall({
				    phoneNumber: self.labelData.description
				});
			},
			
			getProductData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:1
				};
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 4
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.productData.push.apply(self.productData, res.info.data)
					}
					console.log('self.productData', self.productData)
					self.$Utils.finishFunc('getProductData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
		
	};
</script>

<style>
@import "../../assets/style/proList.css";
@import "../../assets/style/navbar.css";
page{padding-bottom: 110rpx;}
button{
	background: none;
	line-height: 1.5;
	margin: 0;
}
button::after{
	border: none;
}
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

