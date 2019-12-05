<template>
	<view>
		<view class="bjPic" style="background: url(../../static/images/home-img0.png) no-repeat 0 0/100% 456rpx;">
			<!-- 搜索 -->
			<view class="seachbox flexRowBetween">
				<view class="flex" >
					<image class="mgr5 Licon" src="../../static/images/home-icon.png" />
					<view class="Gps fs13 avoidOverflow white">西安市</view>
				</view>
				<view class="flexRowBetween rr" style="width: 75%;" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
					<button class="seachBtn" type="button"></button>
					<view class="input">
						<input type="text" name="" value="" placeholder="乌苏 西风 洋河" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
			
			<view class="orderNav flex fs13 white mgb10">
				<view class="tt on" @click="Router.navigateTo({route:{path:'/pages/index/index'}})">热门活动</view>
				<view class="tt" @click="Router.navigateTo({route:{path:'/pages/newProdt/newProdt'}})">新品上市</view>
				<view class="tt" @click="Router.navigateTo({route:{path:'/pages/selected/selected'}})">品类精选</view>
			</view>
		
			<!-- banner -->
			<view class="index_topBj pdlr4">
				<view class="banner-box">
					<view class="banner">
						<swiper class="swiper-box flex" indicator-dots="true" autoplay="true" interval="3000" duration="1000"  indicator-active-color="#FF2121">
							<block v-for="(item,index) in labelData" :key="index">
								<swiper-item class="swiper-item">
									<image :src="item" class="slide-image"/>
								</swiper-item>
							</block>
						</swiper>
					</view>
				</view>
			</view>
		</view>
		
		<view class="mglr4">
			<!-- 分类导航 -->
			<view class="indHome flex pdt15">
				<view class="item"  @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
					<image src="../../static/images/home-icon2.png"></image>
					<view class="tit">白酒</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
					<image src="../../static/images/home-icon3.png"></image>
					<view class="tit">葡萄酒</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
					<image src="../../static/images/home-icon4.png"></image>
					<view class="tit">啤酒</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
					<image src="../../static/images/home-icon5.png"></image>
					<view class="tit">洋酒</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
					<image src="../../static/images/home-icon6.png"></image>
					<view class="tit">黄酒</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
					<image src="../../static/images/home-icon7.png"></image>
					<view class="tit">饮料</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/integralShop/integralShop'}})">
					<image src="../../static/images/home-icon8.png"></image>
					<view class="tit">积分商城</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/nearbyStore/nearbyStore'}})">
					<image src="../../static/images/home-icon9.png"></image>
					<view class="tit">附近门店</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/VIP-Equity/VIP-Equity'}})">
					<image src="../../static/images/home-icon10.png"></image>
					<view class="tit">会员权益</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/My-Wallet/My-Wallet'}})">
					<image src="../../static/images/home-icon11.png"></image>
					<view class="tit">我的钱包</view>
				</view>
			</view>
			
			<view class="pdb15">
				<view  @click="Router.navigateTo({route:{path:'/pages/new-Register/new-Register'}})">
					<image style="width: 100%;height: 193rpx;display: block;margin: 0 auto;" src="../../static/images/home-img.png" mode=""></image>
				</view>
				
			</view>
		
			<!-- 特惠 -->
			<view class="indCoupon center pdlr4" @click="Router.navigateTo({route:{path:'/pages/hongbao/hongbao'}})" style="background:url(../../static/images/home-imgOne.png) no-repeat 0 0/100% 100%;">
				<view class="title">双十一千万红包特惠</view>
				<view class="flex red">
					<view class="item" v-for="(item,index) in couponData" :key="index">
						<view class="mny">{{item.price}}</view>
						<view>{{item.msg}}</view>
						<view>{{item.lable}}</view>
					</view>
				</view>
			</view>
			
			<!--  -->
			<view class="flexRowBetween ind_probox1 pdt20 pdb15 white">
				<view class="Lbox flexColumn" @click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail'}})">
					<image class="FXLabel" src="../../static/images/home-img3.png" mode=""></image>
					<view class="pic"><image src="../../static/images/home-img2.png" mode=""></image></view>
					<view class="pr flexCenter">
						<text class="name">发现好物</text>
						<view class="xian"></view>
					</view>
					<view class="fs10">开启品质新生活</view>
					<view class="goBtn center mgt10">点击进入&gt;</view>
				</view>
				<view class="Rbox">
					<view class="item flexRowBetween mgb5 radius10" @click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail'}})">
						<image class="FXLabel" src="../../static/images/home-img3.png" mode=""></image>
						<view class="left">
							<view class="pr flex">
								<text class="name avoidOverflow">全家人都爱喝</text>
								<view class="xian"></view>
							</view>
							<view class="fs10 avoidOverflow">低至3元起 多买更优惠</view>
							<view class="goBtn center mgt10">点击进入&gt;</view>
						</view>
						<view class="right flex">
							<view class="pic"><image src="../../static/images/home-img2.png" mode=""></image></view>
						</view>
					</view>
					<view class="item flexRowBetween radius10" @click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail'}})">
						<image class="FXLabel" src="../../static/images/home-img3.png" mode=""></image>
						<view class="left">
							<view class="pr flex">
								<text class="name avoidOverflow">全家人都爱喝</text>
								<view class="xian"></view>
							</view>
							<view class="fs10 avoidOverflow">低至3元起 多买更优惠</view>
							<view class="goBtn center mgt10">点击进入&gt;</view>
						</view>
						<view class="right flex">
							<view class="pic"><image src="../../static/images/home-img2.png" mode=""></image></view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="f5H5"></view>
		
		<!-- 啤酒精选 -->
		<view class="mglr4 pdb15">
			<view class="pdt20 pdb10 flexRowBetween">
				<view class="fs15 ftw">啤酒精选</view>
				<view class="color9 flexEnd fs12">查看更多<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
			<view class="flexRowBetween jingxuan pijiu">
				<view class="w460 h300 pr radius10" style="background: #ffffed;"  @click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail'}})">
					<view class="infor">
						<view class="tit">一次喝六国</view>
						<view class="tex2">醇香六国风味啤酒</view>
						<view class="goBtn">立即进入&gt;</view>
					</view>
					<view class="pic">
						<image src="../../static/images/home-img6.png" mode=""></image>
					</view>
					<image class="B-fxBj" src="../../static/images/home-img7.png" alt=""/>
				</view>
				<view class="w220 h300 pr radius10" style="background: #effaec;"  @click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail'}})">
					<view class="infor">
						<view class="tit">潮品试饮</view>
						<view class="tex2">口感更佳</view>
						<view class="goBtn">立即进入&gt;</view>
					</view>
					<view class="pic">
						<image src="../../static/images/home-img9.png" mode=""></image>
					</view>
					<image class="B-fxBj" src="../../static/images/home-img8.png" mode=""/>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<!-- 红酒精选 -->
		<view class="mglr4 pdb15">
			<view class="pdt20 pdb10 flexRowBetween">
				<view class="fs15 ftw">红酒精选</view>
				<view class="color9 flexEnd fs12">查看更多<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
			<view class="flexRowBetween jingxuan hongjiu">
				<view class="w220 h300 pr radius10"  @click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail'}})">
					<view class="infor">
						<view class="tit">一杯红酒</view>
						<view class="tex2">生活需要精致感</view>
						<view class="goBtn">立即进入&gt;</view>
					</view>
					<view class="pic">
						<image src="../../static/images/home-img9.png" mode=""></image>
					</view>
				</view>
				<view class="w460 h300 pr radius10" style="background: #ffffed;"  @click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail'}})">
					<view class="infor">
						<view class="tit">邂逅法国浪漫</view>
						<view class="tex2">圣赛兰葡萄酒优雅而细腻</view>
						<view class="goBtn">立即进入&gt;</view>
					</view>
					<view class="pic">
						<image src="../../static/images/home-img6.png" mode=""></image>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<!-- 白酒精选 -->
		<view class="mglr4 pdb15">
			<view class="pdt20 pdb10 flexRowBetween">
				<view class="fs15 ftw">白酒精选</view>
				<view class="color9 flexEnd fs12">查看更多<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
			
			<view class="flexRowBetween baijiu">
				<view class="w230 pr radius10"  @click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail'}})">
					<view class="infor">
						<view class="tit">一杯红酒</view>
						<view class="tex2">生活需要精致感</view>
					</view>
					<view class="pic flexCenter pdt15 mgt10">
						<image src="../../static/images/home-img2.png" mode=""></image>
					</view>
				</view>
				<view class="w450 pr">
					<view class="item mgb5 one pr radius10" style="background: #ffead8;"  @click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail'}})">
						<view class="infor">
							<view class="tit" style="color: #fc7b0c;">低价品高</view>
							<view class="tex2" style="color: #ff953a;">>人气爆款，喝酒更省心</view>
						</view>
						<view class="pic">
							<image src="../../static/images/home-img2.png" mode=""></image>
						</view>
					</view>
					<view class="item mgb5 two pr radius10" style="background: #d6edff;"  @click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail'}})">
						<view class="infor">
							<view class="tit" style="color: #1a9aff;">宴会专区</view>
							<view class="tex2" style="color: #47aeff;">婚宴、聚会、满月酒</view>
						</view>
						<view class="pic">
							<image src="../../static/images/home-img2.png" mode=""></image>
						</view>
					</view>
					
				</view>
			</view>
		</view>
		
		<!-- 为您推荐 -->
		<view class="pdlr4 f5bj pdb15">
			<view class="pdt20 pdb15 flexRowBetween">
				<view class="fs15 ftw">为您推荐</view>
				<view class="color9 flexEnd fs12">查看更多<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
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
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
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
					<image src="../../static/images/nabar4.png" />
				</view>
				<view class="text">我的</view>
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
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
				],
				couponData:[
					{price:'5',msg:'满50元使用',lable:'(除香烟除外)'},
					{price:'10',msg:'满100元使用',lable:'(除香烟除外)'},
					{price:'15',msg:'满200元使用',lable:'(除香烟除外)'},
					{price:'20',msg:'满300元使用',lable:'(除香烟除外)'},
				],
				proList:[{},{},{},{},{},{}]
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
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/NavTab.css";
	@import "../../assets/style/index.css";
	@import "../../assets/style/seach.css";
	@import "../../assets/style/proList.css";
	page{padding-bottom: 110rpx;} 
	.bjPic{background: url(../../static/images/home-img0.png) no-repeat 0 0/100% 456rpx;}
	.orderNav .tt.on{ color: #fff; position: relative; font-size: 30rpx;}
	.orderNav .tt.on::after{content: ""; width: 60rpx; height: 6rpx;background: #fff; position: absolute; bottom: 0;left: 50%;transform: translateX(-50%);}
	
</style>
