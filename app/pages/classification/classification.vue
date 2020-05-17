<template>
	<view>
		<view style="position: relative;z-index: 90;">
			<!-- 搜索 -->
			<view class="seachbox flexRowBetween whiteBj">
				<view class="flex" >
					<image class="mgr5" style="width: 20rpx; height: 26rpx;" src="../../static/images/home-select-icon.png" />
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
			
			<view class="fs13 pdlr4 borderB1 flexRowBetween whiteBj">
				<view class="orderNav flex" style="width:90% ;">
					<view class="tt" v-for="(item,index) in typeData" :class="currId==item.id?'on':''" 
					@click="changeCurr(item.id)"  :key="index" v-if="index<5">{{item.title}}</view>
				</view>
				<view class="flexEnd navdian" style="width: 10%;" @click="moreClass">
					<span></span>
					<span></span>
					<span></span>
				</view>
			</view>
			<view class="whiteBj category fs13 color9 flexRowBetween borderB1">
				<view class="item flexCenter" @click="zongheShow" :style="orderItem=='zonghe'?'color:#FF2121':''">综合
					<image class="icon" style="width: 20rpx; height:20rpx;" 
					:src="orderItem=='zonghe'?'../../static/images/classification-icon11.png':'../../static/images/classification-icon12.png'" mode=""></image>
				</view>
				<view class="item flexCenter" @click="orderChange('price')" :style="orderItem=='price'?'color:#FF2121':''">价格	
					<image class="icon" style="width: 20rpx; height:20rpx;" 
					:src="orderItem!='price'?'../../static/images/orderNone.png':(order.price&&orderType=='asc'?'../../static/images/orderTop.png':'../../static/images/orderBottom.png')" mode=""></image>
				</view>
				<view class="item flexCenter" @click="orderChange('listorder')" :style="orderItem=='listorder'?'color:#FF2121':''">
					推荐
				<image class="icon" style="width: 20rpx; height:20rpx;"
				:src="orderItem!='listorder'?'../../static/images/orderNone.png':(order.price&&orderType=='asc'?'../../static/images/orderTop.png':'../../static/images/orderBottom.png')" mode=""></image>
				</view>
				
				<view class="item flexCenter" @click="styleShow">
					<image class="icon" style="width: 36rpx; height: 28rpx;" src="../../static/images/classification-icon13.png" mode=""></image>
				</view>
			</view>
			
			<view class="zongheShow whiteBj" v-show="is_zongheShow">
				<view class="item flexRowBetween" :class="num==1?'on':''" @click="changeNum('1')">
					<view class="tt">综合排序</view>
					<image class="selIcon" src="../../static/images/classification-icon10.png" mode=""></image>
				</view>
				<view class="item flexRowBetween" :class="num==2?'on':''" @click="changeNum('2')">
					<view class="tt">销量排序</view>
					<image class="selIcon" src="../../static/images/classification-icon10.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="black-bj" v-show="is_show" ></view>
		<view class="black-bj2" v-show="is_showT" style="z-index: 92;"></view>
		
		<view class="pdlr4 pdt20 pdb15">
			<view class="proList flex" v-show="!is_styleShow">
				<view class="item" v-for="(item,index) in mainData" :key="index" >
					<view class="pic" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">{{item.title}}</view>
						<view class="flex mgb10">
							<view class="lab" v-if="item.combine_no!=''">组合装</view>
						</view>
						<view class="flexRowBetween">
							<view class="price">{{item.price}}</view>
							<view class="adBtn" @click="addCar(index)">+</view>
						</view>
					</view>
				</view>
			</view>
			<view class="proList proList-row" v-show="is_styleShow">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="pic" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">{{item.title}}</view>
						<view class="flex mgb10">
							<view class="lab" v-if="item.combine_no!=''">组合装</view>
						</view>
						<view class="flexRowBetween B-price">
							<view class="price">{{item.price}}</view>
							<view class="adBtn" @click="addCar(index)">+</view>
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
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">分类</view>
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
		
		<!-- 更多分类导航 -->
		<view class="indHome flex pdt15 moreClass pr" v-show="is_moreClass">
			<view class="colseBtn" style="top: auto;bottom: -120rpx;"  @click="moreClass">×</view>
			<view class="item"  v-for="item in typeData" @click="changeCurr(item.id)">
				<image :src="item.bannerImg&&item.bannerImg[0]?item.bannerImg[0].url:''"></image>
				<view class="tit">{{item.title}}</view>
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
				currId:0,
				num:1,
				navDate: ['白酒','葡萄酒','黄酒','啤酒','洋酒'],
				proList:[{},{},{},{},{},{}],
				is_zongheShow:false,
				is_showT:false,
				is_moreClass:false,
				is_styleShow:false,
				mainData:[],
				typeData:[],
				cityIndex:-1,
				allCityData:[],
				order:{},
				orderItem:'zonghe',
				orderType:'asc'
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.id){
				self.id = options.id;
			};
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getTypeData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getAllCity'], self);	
			
		},
		
		methods: {
			
			addCar(index){
				const self = this;
				var array = self.$Utils.getStorageArray('cartData');
				for (var i = 0; i < array.length; i++) {
					if(array[i].id == self.id){
						var target = array[i]
					}
				}
				if(target){
					target.count  = target.count + 1;
					
				}else{
					var target = self.mainData[index];
					target.count = 1;
					target.behavior = 0;
					target.isSelect = true;
				}
				self.$Utils.setStorageArray('cartData', target, ['id','behavior'], 999);
				self.$Utils.showToast('已加入购物车', 'none', 1000);
			},
			
			orderChange(item){
				const self = this;
				self.order = {};
				self.orderItem = item;
				if(self.orderType=='desc'){
					self.orderType='asc'
				}else{
					self.orderType='desc'
				};
				self.order = {
					[item]:self.orderType
				};
				
				console.log('self.order',self.order)
				self.getMainData(true)
			},
			
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
							var findCity = self.$Utils.findItemInArrayOne(res.info.data, ['title'], uni.getStorageSync('city'));
							self.cityData = findCity[1];
							self.cityIndex = findCity[0];
						
							console.log('cityIndex',self.cityIndex)
							self.city_id = self.cityData && self.cityData.id ? self.cityData.id : 4;
							self.$Utils.loadAll(['getTypeData'], self); 
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
						var findCity = self.$Utils.findItemInArrayOne(self.allCityData, ['title'], self.city)
						console.log('findCity',findCity);
						if (findCity) {
							self.cityIndex = findCity[0];
							self.cityData = findCity[1];
							self.city_id = self.cityData && self.cityData.id ? self.cityData.id : 4;
							uni.setStorageSync('city_id',self.city_id);
							uni.setStorageSync('city',self.cityData.title);
							self.$Utils.loadAll(['getTypeData'], self);
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
										self.$Utils.loadAll(['getTypeData'], self);
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
			
			
			getTypeData() {
				const self = this;
				self.typeData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					city_id:uni.getStorageSync('city_id')
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
						self.currId = self.typeData[0].id;
						if(self.id){
							self.currId = self.id;
						}
					}
					console.log('self.typeData', self.typeData)
					self.getMainData()
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				self.mainData = [];
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					category_id:self.currId,
					type:1,
					city_id:uni.getStorageSync('city_id')
				};
				if (JSON.stringify(self.order) != '{}') {
					postData.order = self.$Utils.cloneForm(self.order);
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getTypeData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			changeCurr(index){
				const self = this;
				self.currId = index;
				if(self.is_moreClass){
					self.is_showT = !self.is_showT;
					self.is_moreClass = !self.is_moreClass
				};
				self.getMainData(true)
			},
			
			changeNum(num){
				const self = this;
				self.order = {};
				if(num!=self.num){
					self.num = num;
					self.orderItem = 'zonghe';
					if(self.num==1){
						self.order = {}
					}else if(self.num==2){
						self.order = {
							sale_count:'desc'
						}
					}
					self.getMainData(true)
				};
				self.is_show = !self.is_show
				self.is_zongheShow = !self.is_zongheShow
			},
			zongheShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_zongheShow = !self.is_zongheShow
			},
			styleShow(){
				const self = this;
				self.is_styleShow = !self.is_styleShow;
			},
			moreClass(){
				const self = this;
				self.is_showT = !self.is_showT;
				self.is_moreClass = !self.is_moreClass
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
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	.orderNav .tt{width: auto; margin-right:50rpx;}
	
	.navdian span{width: 10rpx;height: 10rpx;border-radius: 50%;background: #FF2121;margin: 0 4rpx;}
	.category .item{width: 25%; border-right: 1px solid #eee;}
	.category .item:last-child{border-right:0 ;}
	
	.category{padding: 20rpx 4%;}
	.category .item.on{color: #FF2121;}
	.zongheShow{padding: 20rpx 6%;}
	.zongheShow .item{padding: 20rpx 0; color: #666;}
	.zongheShow .item .selIcon{width: 26rpx; height: 19rpx; display: block; display: none;}
	.zongheShow .item.on{color: #222;font-weight: bold;}
	.zongheShow .item.on .selIcon{display: block;}
	
	.moreClass{position: fixed;top: 0;left: 0;right: 0;padding: 30rpx 4%;background: #fff;width: 100%;box-sizing: border-box;z-index: 100;}
	
	.indHome .item{width: 25%;}
	.indHome .item image{ width: 58rpx;height: 58rpx;}
</style>

