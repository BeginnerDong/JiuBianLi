<template>
	<view>
		<view style="position: relative;z-index: 90;">
			<!-- 搜索 -->
			<view class="seachbox flexRowBetween whiteBj">
				<view class="flex" >
					<image class="mgr5" style="width: 20rpx; height: 26rpx;" src="../../static/images/home-select-icon.png" />
					<span class="Gps fs13 avoidOverflow color9">西安市</span>
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
				<view class="item flexCenter" @click="zongheShow">综合<image class="icon" style="width: 16rpx; height: 12rpx;" src="../../static/images/classification-icon11.png" mode=""></image></view>
				<view class="item flexCenter">价格</view>
				<view class="item flexCenter">推荐<image class="icon" style="width: 16rpx; height: 12rpx;" src="../../static/images/classification-icon12.png" mode=""></image></view>
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
							<view class="lab">组合装</view>
						</view>
						<view class="flexRowBetween">
							<view class="price">{{item.price}}</view>
							<view class="adBtn">+</view>
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
							<view class="lab">组合装</view>
						</view>
						<view class="flexRowBetween B-price">
							<view class="price">{{item.price}}</view>
							<view class="adBtn">+</view>
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
			<!-- <view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
				<image src="../../static/images/classification-icon1.png"></image>
				<view class="tit">葡萄酒</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
				<image src="../../static/images/classification-icon2.png"></image>
				<view class="tit">黄酒</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
				<image src="../../static/images/classification-icon3.png"></image>
				<view class="tit">啤酒</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
				<image src="../../static/images/classification-icon4.png"></image>
				<view class="tit">洋酒</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
				<image src="../../static/images/classification-icon5.png"></image>
				<view class="tit">饮料</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
				<image src="../../static/images/classification-icon6.png"></image>
				<view class="tit">酒具</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
				<image src="../../static/images/classification-icon7.png"></image>
				<view class="tit">茶叶</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
				<image src="../../static/images/classification-icon8.png"></image>
				<view class="tit">休闲</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/classification/classification'}})">
				<image src="../../static/images/classification-icon9.png"></image>
				<view class="tit">生鲜</view>
			</view> -->
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
				typeData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getTypeData'], self);
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
			
			
			getTypeData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
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
						self.currId = self.typeData[0].id
					}
					console.log('self.typeData', self.typeData)
					self.getMainData()
				};
				self.$apis.labelGet(postData, callback);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					category_id:self.currId,
					type:1
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
				if(num!=self.num){
					self.num = num
				}
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

