<template>
	<view>
		
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj">
			<view class="flex" >
				<image class="mgr5 Licon" style="width: 20rpx; height: 26rpx;" src="../../static/images/home-select-icon.png" />
				<picker :range="allCityData"
				range-key="title" @change="changeCity">
					<view class="Gps fs13 avoidOverflow">
						{{allCityData[cityIndex]?allCityData[cityIndex].title:''}}
					</view>
				</picker>
			</view>
			<view class="flexRowBetween rr" style="width: 75%;" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text" name="" value="" placeholder="乌苏 西风 洋河" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		<view class="pdlr4 whiteBj">
			<view class="orderNav flex fs13 mgb10 color6">
				<view class="tt" @click="Router.navigateTo({route:{path:'/pages/index/index'}})">热门活动</view>
				<view class="tt on" @click="Router.navigateTo({route:{path:'/pages/newProdt/newProdt'}})">新品上市</view>
				<view class="tt" @click="Router.navigateTo({route:{path:'/pages/selected/selected'}})">品类精选</view>
			</view>
			
			<!-- banner -->
			<view class="index_topBj">
				<view class="banner-box pdb15">
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
		
		
		
		<view class="pdlr4 pdt20 pdb15">
			<view class="proList flex">
				<view class="item" v-for="(item,index) in mainData" :key="index" @click="Router.redirectTo({route:{path:'/pages/prodetail/prodetail'}})">
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
				is_show:false,
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
				],
				proList:[{},{},{},{},{},{},{},{}],
				mainData:[],
				cityIndex:-1,
				allCityData:[],
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getAllCity'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			changeCity(e){
				const self = this;
				console.log('e',e);
				var item = self.allCityData[e.detail.value];
				uni.setStorageSync('city_id',item.id);
				uni.setStorageSync('city',item.title);
				var data = self.$Utils.getStorageArray('cartData');
				for (var i = 0; i < data.length; i++) {
					if(data[i].city_id!=uni.getStorageSync('city_id')){
						self.$Utils.delStorageArray('cartData', data[i], 'id');
					}
				}
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
						if(uni.getStorageSync('city_id')>0){
							var findCity = self.$Utils.findItemInArray(res.info.data, 'title', uni.getStorageSync('city'));
							self.cityData = findCity[1];
							self.cityIndex = findCity[0];
						
							console.log('cityIndex',self.cityIndex)
							self.city_id = self.cityData && self.cityData.id ? self.cityData.id : 4;
							self.$Utils.loadAll(['getMainData'], self); 
						}else{
							self.$Utils.loadAll(['getLocation'], self);	
						}
						self.allCityData.push.apply(self.allCityData, res.info.data);
						console.log('self.allCityData',self.allCityData)
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
							self.$Utils.loadAll(['getMainData'], self);
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
										self.$Utils.loadAll(['getMainData'], self);
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					city_id:uni.getStorageSync('city_id')
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['新品上市']],
						},
						middleKey: 'category_id',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/NavTab.css";
	@import "../../assets/style/seach.css";
	@import "../../assets/style/proList.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	.orderNav .tt{width: auto; margin-right:50rpx;}
	
	/* banner */
	.swiper-box{height: 260rpx;box-sizing: border-box; width: 100%;border-radius: 8rpx; overflow: hidden;}
	.swiper-box image {width: 100%;height: 100%;}
	
	.orderNav .tt.on{ color: #222; position: relative; font-size: 30rpx;}
	.orderNav .tt.on::after{content: ""; width: 60rpx; height: 6rpx;background: #FF2121; position: absolute; bottom: 0;left: 50%;transform: translateX(-50%);}
</style>

