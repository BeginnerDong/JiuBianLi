<template>
	<view>
		<view class="bjPic" :style="'background: url('+allCityData[cityIndex].mainImg[0].url+') no-repeat 0 0/100% 456rpx;'">
			<!-- 搜索 -->
			<view class="seachbox flexRowBetween">
				<view class="flex" >
					<image class="mgr5 Licon" src="../../static/images/home-icon.png" />
					<picker :range="allCityData" 
					range-key="title" @change="changeCity">
						<view class="Gps fs13 avoidOverflow white">{{allCityData[cityIndex].title?allCityData[cityIndex].title:''}}</view>
					</picker>
					
				</view>
				<view class="flexRowBetween rr" style="width: 75%;" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
					<button class="seachBtn" type="button"></button>
					<view class="input">
						<input disabled="true" type="text" name="" value="" placeholder="乌苏 西风 洋河" placeholder-class="placeholder" />
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
							<block v-for="(item,index) in bannerData" :key="index">
								<swiper-item class="swiper-item" :data-url="item.url"
								  @click="Router.navigateTo({route:{path:$event.currentTarget.dataset.url}})">
									<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image"/>
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
				<view class="item"  v-for="item in typeData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/classification/classification?id='+$event.currentTarget.dataset.id}})">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
					<view class="tit">{{item.title}}</view>
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
					<image style="width: 100%;height: 193rpx;display: block;margin: 0 auto;" 
					:src="memberData.mainImg&&memberData.mainImg[0]?memberData.mainImg[0].url:''" mode=""></image>
				</view>
				
			</view>
		
			<!-- 特惠 -->
			<view class="indCoupon center pdlr4" @click="Router.navigateTo({route:{path:'/pages/hongbao/hongbao'}})" style="background:url(../../static/images/home-imgOne.png) no-repeat 0 0/100% 100%;">
				<view class="title">双十一千万红包特惠</view>
				<view class="flex red">
					<view class="item" v-for="(item,index) in couponData" :key="index">
						<view class="mny">{{item.value}}</view>
						<view>满{{item.condition}}元使用</view>
						<!-- <view>{{item.lable}}</view> -->
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
				<view class="w460 h300 pr radius10" style="background: #ffffed;"  :data-id="labelOneData[0].id"
				@click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="infor">
						<view class="tit">{{labelOneData[0]?labelOneData[0].title:''}}</view>
						<view class="tex2">{{labelOneData[0]?labelOneData[0].description:''}}</view>
						<view class="goBtn">立即进入&gt;</view>
					</view>
					<view class="pic">
						<image :src="labelOneData[0]&&labelOneData[0].mainImg&&labelOneData[0].mainImg[0]?
						labelOneData[0].mainImg[0].url:''" mode=""></image>
					</view>
					<image class="B-fxBj" src="../../static/images/home-img7.png" alt=""/>
				</view>
				<view class="w220 h300 pr radius10" style="background: #effaec;"  :data-id="labelOneData[1].id"
				@click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="infor">
						<view class="tit">{{labelOneData[1]?labelOneData[1].title:''}}</view>
						<view class="tex2">{{labelOneData[1]?labelOneData[1].description:''}}</view>
						<view class="goBtn">立即进入&gt;</view>
					</view>
					<view class="pic">
						<image :src="labelOneData[1]&&labelOneData[1].mainImg&&labelOneData[1].mainImg[0]?
						labelOneData[1].mainImg[0].url:''" mode=""></image>
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
				<view class="w220 h300 pr radius10"  :data-id="labelTwoData[0].id"
				@click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="infor">
						<view class="tit">{{labelTwoData[0]?labelTwoData[0].title:''}}</view>
						<view class="tex2">{{labelTwoData[0]?labelTwoData[0].description:''}}</view>
						<view class="goBtn">立即进入&gt;</view>
					</view>
					<view class="pic">
						<image :src="labelTwoData[0]&&labelTwoData[0].mainImg&&labelTwoData[0].mainImg[0]?
						labelTwoData[0].mainImg[0].url:''" mode=""></image>
					</view>
				</view>
				<view class="w460 h300 pr radius10" style="background: #ffffed;"  :data-id="labelTwoData[1].id"
				@click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="infor">
						<view class="tit">{{labelTwoData[1]?labelTwoData[1].title:''}}</view>
						<view class="tex2">{{labelTwoData[1]?labelTwoData[1].description:''}}</view>
						<view class="goBtn">立即进入&gt;</view>
					</view>
					<view class="pic">
						<image :src="labelTwoData[1]&&labelTwoData[1].mainImg&&labelTwoData[1].mainImg[0]?
						labelTwoData[1].mainImg[0].url:''" mode=""></image>
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
				<view class="w230 pr radius10"  :data-id="labelThreeData[0].id"
				@click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="infor">
						<view class="tit">{{labelThreeData[0]?labelThreeData[0].title:''}}</view>
						<view class="tex2">{{labelThreeData[0]?labelThreeData[0].description:''}}</view>
					</view>
					<view class="pic flexCenter pdt15 mgt10">
						<image :src="labelThreeData[0]&&labelThreeData[0].mainImg&&labelThreeData[0].mainImg[0]?
						labelThreeData[0].mainImg[0].url:''" mode=""></image>
					</view>
				</view>
				<view class="w450 pr">
					<view class="item mgb5 one pr radius10" style="background: #ffead8;"  :data-id="labelThreeData[1].id"
					@click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="infor">
							<view class="tit" style="color: #fc7b0c;">{{labelThreeData[1]?labelThreeData[1].title:''}}</view>
							<view class="tex2" style="color: #ff953a;">{{labelThreeData[1]?labelThreeData[1].description:''}}</view>
						</view>
						<view class="pic">
							<image :src="labelThreeData[1]&&labelThreeData[1].mainImg&&labelThreeData[1].mainImg[0]?
						labelThreeData[1].mainImg[0].url:''" mode=""></image>
						</view>
					</view>
					<view class="item mgb5 two pr radius10" style="background: #d6edff;"  :data-id="labelThreeData[2].id"
					@click="Router.navigateTo({route:{path:'/pages/jingxuanDetail/jingxuanDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="infor">
							<view class="tit" style="color: #1a9aff;">{{labelThreeData[2]?labelThreeData[2].title:''}}</view>
							<view class="tex2" style="color: #47aeff;">{{labelThreeData[2]?labelThreeData[2].description:''}}</view>
						</view>
						<view class="pic">
							<image :src="labelThreeData[2]&&labelThreeData[2].mainImg&&labelThreeData[2].mainImg[0]?
						labelThreeData[2].mainImg[0].url:''" mode=""></image>
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
			
				proList:[{},{},{},{},{},{}],
				typeData:[],
				labelOneData:[],
				labelTwoData:[],
				labelThreeData:[],
				memberData:{},
				couponData:[],
				productData:[],
				bannerData:[],
				city:'',
				allCityData:[],
				cityIndex:-1
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.user_no){
				uni.setStorageSync('parent_no',options.user_no)
			};
			self.$Utils.loadAll(['getMemberData','getCouponData'], self);	
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getAllCity'], self);	
			
		},
		
		methods: {
			
			changeCity(e){
				const self = this;
				console.log('e',e);
				var item = self.allCityData[e.detail.value];
				uni.setStorageSync('city_id',item.id);
				uni.setStorageSync('city',item.title);
				self.$Utils.loadAll(['getAllCity'], self);	
			},
			
			getAllCity() {
				const self = this;
				self.allCityData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:7
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.allCityData.push.apply(self.allCityData, res.info.data)
						if(uni.getStorageSync('city_id')>0){
							var findCity = self.$Utils.findItemInArray(self.allCityData, 'title', uni.getStorageSync('city'));
							self.cityData = findCity[1];
							self.cityIndex = findCity[0];
							console.log('cityIndex',self.cityIndex)
							self.city_id = self.cityData && self.cityData.id ? self.cityData.id : 4;
							self.$Utils.loadAll(['getBannerData','getTypeData','getLabelOneData','getLabelTwoData','getLabelThreeData','getProductData'], self); 
						}else{
							self.$Utils.loadAll(['getLocation'], self);	
						}
					}
					self.$Utils.finishFunc('getAllCity');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getLocation() {
				const self = this;
				const callback = (res) => {
					if (res) {
						console.log('res', res)
						if (res.authSetting) {
							console.log(232)
							return
						}
						self.city = res.address_component.city
						var findCity = self.$Utils.findItemInArray(self.allCityData, 'title', self.city)
						console.log('findCity',findCity);
						if (findCity) {
							self.cityIndex = findCity[0];
							self.cityData = findCity[1];
							self.city_id = self.cityData && self.cityData.id ? self.cityData.id : 4;
							uni.setStorageSync('city_id',self.city_id);
							uni.setStorageSync('city',self.cityData.title);
							self.$Utils.loadAll(['getBannerData','getTypeData','getLabelOneData','getLabelTwoData','getLabelThreeData','getProductData'], self);
						} else {
							uni.showModal({
								title: '提示',
								content: '当前城市未开通，是否切换为默认城市西安',
								success: function(res) {
									if (res.confirm) {
										self.city_id = self.cityData && self.cityData.id ? self.cityData.id : 4;
										self.city = '西安市';
										uni.setStorageSync('city_id',self.city_id);
										uni.setStorageSync('city','西安市');
										self.$Utils.loadAll(['getTypeData','getLabelOneData','getLabelTwoData','getLabelThreeData','getProductData'], self);
									} else if (res.cancel) {
										console.log('用户点击取消');
									}
								}
							});
						};
					};
				};
				self.$Utils.getLocation('reverseGeocoder', callback);
			
			},
			
			getCityData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					
					title: self.city
				};
				const callback = (res) => {
					self.$Utils.finishFunc('getLocation');
					if (res.info.data.length > 0) {
						self.cityData = res.info.data[0];
						self.city_id = self.cityData && self.cityData.id ? self.cityData.id : 4;
						uni.setStorageSync('city_id',self.city_id);
						uni.setStorageSync('city',self.cityData.title);
						self.$Utils.loadAll(['getBannerData','getTypeData','getLabelOneData','getLabelTwoData','getLabelThreeData','getProductData'], self);	
					} else {
						uni.showModal({
							title: '提示',
							content: '当前城市未开通，是否切换为默认城市西安',
							success: function(res) {
								if (res.confirm) {
									self.city_id = self.cityData && self.cityData.id ? self.cityData.id : 4;
									self.city = '西安市';
									uni.setStorageSync('city_id',self.city_id);
									uni.setStorageSync('city','西安市');
									self.$Utils.loadAll(['getTypeData','getLabelOneData','getLabelTwoData','getLabelThreeData','getProductData'], self);
								} else if (res.cancel) {
									console.log('用户点击取消');
								}
							}
						});
					}
			
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getBannerData() {
				const self = this;
				self.bannerData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					city_id:self.city_id
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['首页轮播']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.bannerData.push.apply(self.bannerData, res.info.data)
					}
					console.log('self.bannerData', self.bannerData)
					self.$Utils.finishFunc('getBannerData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMemberData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					title:'注册会员'
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.memberData = res.info.data[0];
					}
					self.$Utils.finishFunc('getMemberData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getTypeData() {
				const self = this;
				self.typeData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					city_id:self.city_id
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['分类']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.typeData.push.apply(self.typeData, res.info.data);
						
					}
					console.log('self.typeData', self.typeData)
					self.$Utils.finishFunc('getTypeData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getLabelOneData() {
				const self = this;
				self.labelOneData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					city_id:self.city_id
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['啤酒精选']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelOneData.push.apply(self.labelOneData, res.info.data)
					}
					console.log('self.labelOneData', self.labelOneData)
					self.$Utils.finishFunc('getLabelOneData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getLabelTwoData() {
				const self = this;
				self.labelTwoData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					city_id:self.city_id
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['红酒精选']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelTwoData.push.apply(self.labelTwoData, res.info.data)
					}
					console.log('self.labelTwoData', self.labelTwoData)
					self.$Utils.finishFunc('getLabelTwoData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getLabelThreeData() {
				const self = this;
				self.labelThreeData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					city_id:self.city_id
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['白酒精选']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelThreeData.push.apply(self.labelThreeData, res.info.data)
					}
					console.log('self.labelThreeData', self.labelThreeData)
					self.$Utils.finishFunc('getLabelThreeData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getCouponData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					
				};
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData.push.apply(self.couponData, res.info.data)
					}
					console.log('self.couponData', self.couponData)
					self.$Utils.finishFunc('getCouponData');
				};
				self.$apis.couponGet(postData, callback);
			},
			
			getProductData() {
				const self = this;
				self.productData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					city_id:self.city_id,
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
