<template>
	<view>
		
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj">
			<view class="flex" >
				<image class="mgr5 Licon" style="width: 20rpx; height: 26rpx;" src="../../static/images/home-select-icon.png" />
				<view class="Gps fs13 avoidOverflow color6">印象长安</view>
			</view>
			<view class="flexRowBetween rr" style="width: 75%;" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text" name="" value="" placeholder="乌苏 西风 洋河" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		<view class="pdlr4 whiteBj">
			<view class="orderNav flex fs13 pdb10 color6">
				<view class="tt" @click="Router.navigateTo({route:{path:'/pages/index/index'}})">热门活动</view>
				<view class="tt" @click="Router.navigateTo({route:{path:'/pages/newProdt/newProdt'}})">新品上市</view>
				<view class="tt on" @click="Router.navigateTo({route:{path:'/pages/selected/selected'}})">品类精选</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		
		<!-- 分类导航 -->
		<view class="indHome flex  pdt15  whiteBj">
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/selectedClassify/selectedClassify'}})">
				<image src="../../static/images/home-select-icon1.png"></image>
				<view class="tit">甄选</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/selectedClassify/selectedClassify'}})">
				<image src="../../static/images/home-select-icon2.png"></image>
				<view class="tit">白酒</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/selectedClassify/selectedClassify'}})">
				<image src="../../static/images/home-select-icon3.png"></image>
				<view class="tit">葡萄酒</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/selectedClassify/selectedClassify'}})">
				<image src="../../static/images/home-select-icon4.png"></image>
				<view class="tit">啤酒</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/selectedClassify/selectedClassify'}})">
				<image src="../../static/images/home-select-icon5.png"></image>
				<view class="tit">洋酒</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/selectedClassify/selectedClassify'}})">
				<image src="../../static/images/home-select-icon6.png"></image>
				<view class="tit">饮料</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/selectedClassify/selectedClassify'}})">
				<image src="../../static/images/home-select-icon7.png"></image>
				<view class="tit">黄酒</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/selectedClassify/selectedClassify'}})">
				<image src="../../static/images/home-select-icon8.png"></image>
				<view class="tit">全部</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="interct_idexLis">
			<view class="child"  @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail'}})">
				<view class="flex">
					<view class="photo"><image src="../../static/images/home-select-img.png" mode=""></image></view>
					<view class="font13 name">信远斋</view>
				</view>
				<view class="font14 pdtb15">真材细做，货真价实-信远斋桂花酸梅汤</view>
				<view class="imgbox">
					<view class="lisOne">
						<image src="../../static/images/home-select-img1.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="child"  @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail'}})">
				<view class="flex">
					<view class="photo"><image src="../../static/images/home-select-img.png" mode=""></image></view>
					<view class="font13 name">信远斋</view>
				</view>
				<view class="font14 pdtb15">真材细做，货真价实-信远斋桂花酸梅汤</view>
				<view class="imgbox">
					<view class="lisTwo">
						<image src="../../static/images/home-select-img1.png" mode=""></image>
					</view>
					<view class="lisTwo">
						<image src="../../static/images/stoers-img.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="child"  @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail'}})">
				<view class="flex">
					<view class="photo"><image src="../../static/images/home-select-img.png" mode=""></image></view>
					<view class="font13 name">信远斋</view>
				</view>
				<view class="font14 pdtb15">真材细做，货真价实-信远斋桂花酸梅汤</view>
				<view class="imgbox">
					<view class="lisThree">
						<image src="../../static/images/home-select-img1.png" mode=""></image>
					</view>
					<view class="lisThree">
						<image src="../../static/images/stoers-img.png" mode=""></image>
					</view>
					<view class="lisThree">
						<image src="../../static/images/home-select-img1.png" mode=""></image>
					</view>
				</view>
			</view>
		</view>
		
		<view class="pdlr4">
			<view class="pdb10 pdt5"><image class="w" src="../../static/images/home-select-img2.png" mode="widthFix"></image></view>
		</view>
		
		<view class="interct_idexLis">
			<view class="child"  @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail'}})">
				<view class="flex">
					<view class="photo"><image src="../../static/images/home-select-img.png" mode=""></image></view>
					<view class="font13 name">信远斋</view>
				</view>
				<view class="font14 pdtb15">真材细做，货真价实-信远斋桂花酸梅汤</view>
				<view class="imgbox">
					<view class="lisThree">
						<image src="../../static/images/home-select-img1.png" mode=""></image>
					</view>
					<view class="lisThree">
						<image src="../../static/images/stoers-img.png" mode=""></image>
					</view>
					<view class="lisThree">
						<image src="../../static/images/home-select-img1.png" mode=""></image>
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
				is_show:false
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
	@import "../../assets/style/seach.css";
	@import "../../assets/style/selected.css";
	
	page{padding-bottom: 130rpx;background: #F5F5F5;}
	.orderNav .tt{width: auto; margin-right:50rpx;}
	
	/* banner */
	.swiper-box{height: 260rpx;box-sizing: border-box; width: 100%;border-radius: 8rpx; overflow: hidden;}
	.swiper-box image {width: 100%;height: 100%;}
	
	.orderNav .tt.on{ color: #222; position: relative; font-size: 30rpx;}
	
	/* 菜单 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 25%;text-align: center;padding-bottom: 15px; font-size: 24rpx; color: #222;display: flex; flex-direction: column;}
	.indHome .item image {width:90rpx;height: 90rpx; border-radius: 50%;margin-bottom: 20rpx;margin: 0 auto 10rpx auto;}
	

	
</style>

