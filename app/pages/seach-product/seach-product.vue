<template>
	<view>
		<view style="position: relative;z-index: 90;">
			<!-- 搜索 -->
			<view class="seachbox flexRowBetween whiteBj borderB1" style="padding-bottom: 30rpx;">
				<view class="flex rr" style="width: 85%;">
					<button class="seachBtn" type="button"></button>
					<view class="input">
						<input v-model="keywords" type="text" name="" value="" placeholder="乌苏 西风 洋河" placeholder-class="placeholder" />
					</view>
					<view class="delt flex" v-if="keywords!=''" @click="deleteText"><text>×</text></view>
				</view>
				<view class="Rseach pubColor fs15" @click="search">搜索</view>
			</view>
			<view class="whiteBj category fs13 color9 flexRowBetween borderB1">
				<view class="item  flexCenter" :class="zonghe?'on':''" @click="zongheShow">综合</view>
				<view class="item flexCenter" :class="price?'on':''" @click="priceShow">价格</view>
				<view class="item flexCenter" :class="tuijian?'on':''" @click="changeOrder">推荐</view>
				<view class="item flexCenter"  @click="styleShow">
					<image class="icon" style="width: 36rpx; height: 28rpx;" src="../../static/images/classification-icon13.png" mode=""></image>
				</view>
			</view>
			<view class="zongheShow whiteBj" v-show="is_zongheShow">
				<view class="item flexRowBetween"  :class="num==1?'on':''"  @click="changeNum('1')">
					<view class="tt">综合排序{{num}}</view>
					<image class="selIcon" src="../../static/images/classification-icon10.png" mode=""></image>
				</view>
				<view class="item flexRowBetween" :class="num==2?'on':''" @click="changeNum('2')">
					<view class="tt">销量排序</view>
					<image class="selIcon" src="../../static/images/classification-icon10.png" mode=""></image>
				</view>
			</view>
			
			<view class="zongheShow whiteBj" v-show="is_priceShow">
				<view class="item flexRowBetween" :class="num==1?'on':''" @click="changePrice('1')">
					<view class="tt">由高到低</view>
					<image class="selIcon" src="../../static/images/classification-icon10.png" mode=""></image>
				</view>
				<view class="item flexRowBetween" :class="num==2?'on':''" @click="changePrice('2')">
					<view class="tt">由低到高</view>
					<image class="selIcon" src="../../static/images/classification-icon10.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="black-bj" v-show="is_show"></view>
		
		<view class="pdlr4 pdt20 pdb15">
			<view class="proList flex" v-show="!is_styleShow">
				<view class="item" v-for="(item,index) in mainData" :key="index" >
					<view class="pic" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">{{item.title}}</view>
						<view class="flex mgb10" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">
							<view class="lab" v-if="item.combine_no!=''">组合装</view>
						</view>
						<view class="flexRowBetween">
							<view class="price" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">{{item.price}}</view>
							<view class="adBtn" @click="addCar(index)">+</view>
						</view>
					</view>
				</view>
			</view>
		
			<view class="proList proList-row" v-show="is_styleShow">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index" 
				>
					<view class="pic" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">{{item.title}}</view>
						<view class="flex mgb10" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">
							<view class="lab" v-if="item.combine_no!=''">组合装</view>
						</view>
						<view class="flexRowBetween B-price">
							<view class="price" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">{{item.price}}</view>
							<view class="adBtn" @click="addCar(index)">+</view>
						</view>
					</view>
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
				proList:[{},{},{},{},{},{}],
				is_zongheShow:false,
				is_priceShow:false,
				num:1,
				is_styleShow:false,
				mainData:[],
				keywords:'',
				tuijian:false,
				price:false,
				zonghe:true
			}
		},
		
		onLoad(options) {
			const self = this;
			self.keywords = options.keywords;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
			
			search(){
				const self = this;
				if(self.keywords!=''){
					self.getMainData(true)
				}else{
					self.$Utils.showToast('请输入关键词搜索', 'none');
				}
			},
			
			zongheShow(){
				const self = this;
				//self.num = -1;
				self.is_show = true;
				self.is_zongheShow = true
				self.is_priceShow = false
			},
			
			priceShow(){
				const self = this;
				//self.num = -1;
				self.is_show = true;
				self.is_zongheShow = false
				self.is_priceShow = true
			},
			
			changeOrder(){
				const self = this;
				self.price = false;
				self.tuijian = true;
				self.zonghe = false;
				self.order = {
					listorder:'desc'
				};
				self.getMainData(true)
			},
			
			changeNum(num){
				const self = this;
				self.price = false;
				self.tuijian = false;
				self.zonghe = true;
				if(num!=self.num){
					self.num = num;
					if(num==1){
						self.order = {}
					}else if(num==2){
						self.order = {
							sale_count:'desc'
						}
					}
					self.is_show = !self.is_show
					self.is_zongheShow = !self.is_zongheShow
					self.getMainData(true)
				};
			},
			
			changePrice(num){
				const self = this;
				self.price = true;
				self.tuijian = false;
				self.zonghe = false;
				if(num!=self.num){
					self.num = num;
					if(num==1){
						self.order = {
							price:'desc'
						}
					}else if(num==2){
						self.order = {
							price:'asc'
						}
					}
					self.is_show = !self.is_show
					self.is_priceShow = !self.is_priceShow
					self.getMainData(true)
				};
			},
			
			styleShow(){
				const self = this;
				self.is_styleShow = !self.is_styleShow;
			},
			
			deleteText(){
				const self = this;
				self.keywords = ''
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					title:['LIKE',['%'+self.keywords+'%']]
				};
				if(JSON.stringify(self.order)!='{}'){
					postData.order = self.order
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
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
	@import "../../assets/style/seach.css";
	@import "../../assets/style/proList.css";
	page{background: #F5F5F5;}
	
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
	
</style>

