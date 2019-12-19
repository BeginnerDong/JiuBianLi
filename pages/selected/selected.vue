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
			<view class="item" v-for="item in typeData" 
			 @click="Router.navigateTo({route:{path:'/pages/selectedClassify/selectedClassify'}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
				<view class="tit">{{item.title}}</view>
			</view>
			
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/selectedClassify/selectedClassify'}})">
				<image src="../../static/images/home-select-icon8.png"></image>
				<view class="tit">全部</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="interct_idexLis">
			<view class="child"  v-for="item in mainData" 
			>
				<view class="flex" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="photo">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="font13 name">{{item.title}}</view>
				</view>
				<view class="font14 pdtb15" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail?id='+$event.currentTarget.dataset.id}})">真材细做，货真价实-信远斋桂花酸梅汤</view>
				<view class="imgbox">
					<view v-for="(c_item,c_index) in item.bannerImg" :class="item.bannerImg.length==1?'lisOne':(item.bannerImg.length==2?'lisTwo':'lisThree')">
						<image :src="c_item.url" :data-index="c_index"  :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail?id='+$event.currentTarget.dataset.id+'&index='+$event.currentTarget.dataset.index}})" mode="aspectFill"></image>
					</view>
				</view>
			</view>
			
		</view>
		
		<!-- <view class="pdlr4">
			<view class="pdb10 pdt5"><image class="w" src="../../static/images/home-select-img2.png" mode="widthFix"></image></view>
		</view> -->

		
		
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
				typeData:[],
				idArray:[],
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
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
							title: ['=', ['品类精选']],
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
						for (var i = 0; i < self.typeData.length; i++) {
							self.idArray.push(self.typeData[i].id)
						}
					}
					self.getMainData()
					console.log('self.typeData', self.typeData)
					self.$Utils.finishFunc('getTypeData');
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
					menu_id:['in',self.idArray]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					
				};
				self.$apis.articleGet(postData, callback);
			},
			
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

