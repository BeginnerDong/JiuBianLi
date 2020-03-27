<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj">
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button"></button>
				<view class="input" style="width: 85%;">
					<input type="text" name="" v-model="address" placeholder="搜索地址/街道" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="Rseach pubColor fs15" @click="search">搜索</view>
		</view>
		
		<view class="storeList pdlr4">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="photo"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
				<view class="infor" @click="choose(index)">
					<view class="flexRowBetween">
						<view class="title avoidOverflow">{{item.name}}</view>
						<view class="fs12 color6">{{item.distance}}km</view>
					</view>
					<view class="adrs fs12 color6">{{item.address}}</view>
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
				storeList:[{},{},{},{}],
				mainData:[],
				searchItem:{
					thirdapp_id: 2,
					user_type:1
				},
				address:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLocation'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		onPullDownRefresh() {
			console.log('refresh');
			const self = this;
			self.address = '';
			if(self.searchItem.address!=''){
				delete self.searchItem.address;
				
			};
			self.getMainData(true)
			
		},
		
		methods: {
			
			choose(index) {
				const self = this;
				self.choosedIndex = index;
				uni.setStorageSync('chooseShopData', self.mainData[index]);
				console.log('choosedIndex', self.choosedIndex);
				uni.navigateBack({
					delta:1
				})
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
						self.lng = res.location.lng;
						self.lat = res.location.lat
						self.getMainData(true)
					};
				};
				self.$Utils.getLocation('reverseGeocoder', callback);
			},
			
			search(){
				const self = this;
				if(self.address!=''){
					self.searchItem.address = ['LIKE',['%'+self.address+'%']]
					self.getMainData(true)
				}else{
					self.$Utils.showToast('请输入地址搜索','none')
				}
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5
					}
				};
				const postData = {};
				postData.getBefore = {
					shop: {
						tableName: 'User',
						searchItem: {
							city_id: ['=', [uni.getStorageSync('city_id')]],
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
					},
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				postData.order = {
					distance:'asc',
					longitude:self.lng,
					latitude:self.lat,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].distance =
								self.$Utils.distance(self.lat, self.lng, self.mainData[i].latitude, self.mainData[i].longitude)
							
						}
					};
					setTimeout(function () {
						uni.stopPullDownRefresh();
					}, 1000);
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getLocation');
				};
				self.$apis.userInfoGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/seach.css";
	.storeList .item{align-items: flex-start;padding: 30rpx 0; border-bottom: 1px solid #eee;}
	.storeList .item .photo{width: 180rpx; height: 140rpx;}
	.storeList .item .photo image{width: 100%; height: 100%; display: block;}
	.storeList .item .infor{width: 70%; height: 140rpx;position: relative;}
	.storeList .item .title{width: 75%;}
	.storeList .item .adrs{position: absolute; bottom: 8rpx;left: 0;right: 0;line-height: 32rpx;}
</style>

