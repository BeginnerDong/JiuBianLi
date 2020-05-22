<template>
	<view>
		<view class="orderNav flex fs13 borderB1">
			<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">商品</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">评论</view>
			<view  class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">详情</view>
		</view>
		
		<!-- 商品 -->
		<view class="" v-if="curr==1">
			<!-- banner -->
			<view class="banner-box">
				<swiper class="swiper-box flex" indicator-dots="true" autoplay="true" interval="3000" duration="1000"  indicator-active-color="#FF2121">
					<block v-for="(item,index) in mainData.bannerImg" :key="index">
						<swiper-item class="swiper-item">
							<image :src="item.url" class="slide-image"/>
						</swiper-item>
					</block>
				</swiper>
			</view>
			<view class="pdlr4 mgt10 mgb10">
				<view class="mgb5">{{mainData.title}}</view>
				<view class="flex">
					<view class="price">{{mainData.price}}</view>
					<view class="yuanJia mgl15">{{mainData.o_price}}</view>
				</view>
			</view>
			<view class="f5H5"></view>
			<view class="mglr4">
				<view class="pdt15 pdb15 flexRowBetween borderB1"  @click="couponShow">
					<view class="flex">
						<view class="ftw">领券</view>
						<view class="flex fs12 center C-canshu lingQuan">
							<view class="item" v-for="(item,index) in couponData" v-if="index<3">满{{item.condition}}减{{item.value}}</view>
						</view>
					</view>
					<view><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				
				<view class="pdt15 pdb15 flexRowBetween" v-if="mainData.combine_price!=''" 
				@click="Router.navigateTo({route:{path:'/pages/promotion/promotion?id='+mainData.id}})">
					<view class="flex">
						<view class="ftw mgr15">促销</view>
						<view class="flex fs12 C-canshu">
							<view class="cux-Labe mgr10">组合购</view>
							<view class="avoidOverflow" style="width: 400rpx;">{{mainData.title}} {{mainData.combine_price}}/{{mainData.combine_count}}瓶</view>
						</view>
					</view>
					<view><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
			</view>
			<view class="f5H5"></view>
			
			<view class="mglr4 pdt15 pdb15 flexRowBetween"  @click="specsShow">
				<view class="flex">
					<view class="ftw mgr15">规格</view>
					<view class="flex fs13 C-canshu color6">
						<view class="avoidOverflow">品牌、产地产区、规格、酒精度、净含量</view>
					</view>
				</view>
				<view><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
			<view class="f5H5"></view>
			<view class="mglr4 pingjia">
				<view class="flexRowBetween pdt15 pdb15">
					<view class="flex">
						<view class="ftw mgr15">评价</view>
						<view class="fs12 color6">{{totalMessage}}人</view>
					</view>
					<view class="color6 fs12 flexEnd">好评度{{goodRate}}%<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				<view  v-if="messageData.length>0">
					<view class="flexRowBetween pdb15">
						<view class="flex">
							<image class="us-photo mgr10" 
							:src="messageData[0].mainImg&&messageData[0].mainImg[0]?messageData[0].mainImg[0].url:''" mode=""></image>
							<view class="color6">
								<view class="fs13">{{messageData[0].title}}</view>
								<view class="fs10">{{messageData[0].create_time}}</view>
							</view>
						</view>
						<view class="xing flexEnd">
							<view class="starBox mgr5">
								<image v-for="c_item in stars" :src="messageData[0].score/2 > c_item ?(messageData[0].score/2-c_item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">
								
								</image>
							</view>
							<view class="fs12 color9">{{messageData[0].score}}分</view>
						</view>
					</view>
					<view class="fs13">{{messageData[0].description}}</view>
					<view class="flexCenter" @click="changeCurr('2')">
						<view class="moreBtn mgt20 mgb20 fs12">更多评价</view>
					</view>
				</view>
				<view v-else style="text-align: center;">暂无评论~</view>
			</view>
			<view class="f5H5"></view>
			<view class="mglr4 pdt15 pdb15">
				<view class="ftw mgr15 pdb10">温馨提示</view>
				<view class="xqInfor fs12">
					<view>非酒水包装等问题，酒水一旦销售将不予退换</view>
					<view>避免光照，通风地方存放</view>
					<view>由于部分商品包装更换较为频繁，请您已收到的商品实物为准，图片仅供参考</view>
				</view>
			</view>
			<view class="f5H5"></view>
			<view class="Promise flexRowBetween mglr4 fs12">
				<view class="item flexCenter">
					<image class="icon" src="../../static/images/details-icon4.png" mode=""></image>
					<text>正品保证</text>
				</view>
				<view class="item flexCenter">
					<image class="icon" src="../../static/images/details-icon5.png" mode=""></image>
					<text>百家自营门店</text>
				</view>
			</view>
			<view class="f5bj pdlr4">
				<view class="titbj flexCenter fs15" style="color: #c9850f">猜你喜欢</view>
				
				<view class="f5bj pdb15">
					<view class="proList flex">
						<view class="item" v-for="(item,index) in mainData.relationProduct" :key="index" :data-id="item.id"
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
			</view>
		</view>
		<!-- 商品 end -->
		
		<!-- 评论 -->
		<view class="" v-if="curr==2">
			<view class="pingjia">
				<view class="flexRowBetween pdt15 mglr4">
					<view class="flex">
						<view class="ftw mgr15">评价</view>
						<view class="fs12 color6">{{totalMessage}}人</view>
					</view>
					<view class="color6 fs12 flexEnd">好评度{{goodRate}}%<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				<view class="pjList" v-for="(item,index) in messageData" :key="index">
					<view class="flexRowBetween pdb15">
						<view class="flex">
							<image class="us-photo mgr10" 
							:src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
							<view class="color6">
								<view class="fs13">{{item.title}}</view>
								<view class="fs10">{{item.create_time}}</view>
							</view>
						</view>
						<view class="xing flexEnd">
						
							<view class="starBox mgr5">
								<image v-for="c_item in stars" :src="item.score/2 > c_item ?(item.score/2-c_item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">
								
								</image>
							</view>
							<view class="fs12 color9">{{item.score}}分</view>
						</view>
					</view>
					<view class="fs13">{{item.description}}</view>
				</view>
				
			</view>
		</view>
		
		<!-- 详情 -->
		<view class="pdt5" v-if="curr==3">
			<view class="content ql-editor" style="padding:0;line-height: 0;" v-html="mainData.content">
			</view>
		</view>
		
		<view class="black-bj" v-if="is_show"></view>
		
		<!-- 优惠券弹框 -->
		<view class="couponShow f5bj pdlr4" v-if="is_couponShow">
			<view class="closebtn fs18 color6 pdr10 pdl10 mgt5" @click="couponShow">×</view>
			<view class="pdt15 pdb15 center">可领优惠券</view>
			<view class="couponlist list">
				<view class="item flexRowBetween" v-for="(item,index) in couponData" :key="index">
					<view class="flex ll">
						<view class="mny">{{item.value}}</view>
						<view class="infor fs12">
							<view>{{item.value}}元优惠券</view>
							<view class="flex mgt15"><view class="labBtn">满{{item.condition}}元可用</view></view>
						</view>
					</view>
					<view class="rr flexEnd">
						<view class="lq-btn" @click="submit(index)">领取</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 规格弹框 -->
		<view class="specsShow whiteBj pdlr4" v-if="is_specsShow">
			<view class="closebtn fs18 color6 pdr10 pdl10 mgt5" @click="specsShow">×</view>
			<view class="pdt15 pdb15 center borderB1">规格</view>
			<view class="editLine fs13">
				<view class="item pdtb10 borderB1 flex">
					<view class="ll color6">品牌</view>
					<view class="rr">{{mainData.brand}}</view>
				</view>
				<view class="item pdtb10 borderB1 flex">
					<view class="ll color6">产地产区</view>
					<view class="rr">{{mainData.origin}}</view>
				</view>
				<view class="item pdtb10 borderB1 flex">
					<view class="ll color6">规格</view>
					<view class="rr">{{mainData.specifications}}</view>
				</view>
				<view class="item pdtb10 borderB1 flex">
					<view class="ll color6">酒精度</view>
					<view class="rr">{{mainData.alcohol}}</view>
				</view>
				<view class="item pdtb10 borderB1 flex">
					<view class="ll color6">净含量</view>
					<view class="rr">{{mainData.net_content}}</view>
				</view>
				<view class="item pdtb10 borderB1 flex">
					<view class="ll color6">香型</view>
					<view class="rr">{{mainData.aroma}}</view>
				</view>
				<view class="item pdtb10 borderB1 flex">
					<view class="ll color6">执行标准</view>
					<view class="rr">{{mainData.standard}}</view>
				</view>
				<view class="item pdtb10 borderB1 flex">
					<view class="ll color6">储存条件</view>
					<view class="rr">{{mainData.storage}}</view>
				</view>
			</view>
		</view>
			
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween" v-if="!is_carNumShow">
			<view class="left flexRowBetween">
				<view class="ite" @click="collect()">
					<image :src="mainData.collectMe&&mainData.collectMe.length>0&&mainData.collectMe[0].status==1?
					'../../static/images/details-icon7-a.png':'../../static/images/details-icon7.png'" mode=""></image>
					<view>{{mainData.collectMe?mainData.collect.length:''}}人</view>
				</view>
				<view class="ite" @click="callPhone">
					<image src="../../static/images/details-icon8.png" mode=""></image>
					<view>客服</view>
				</view>
				<view class="ite" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
					<image src="../../static/images/details-icon9.png" mode=""></image>
					<view>购物车</view>
				</view>
			</view>
			<view class="payBtn fs16 white" @click="carNumShow">加入购物车</view>
		</view>
		<!-- 底部菜单按钮 end -->
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween" v-if="is_carNumShow">
			<view class="left flexRowBetween">
				<view class="ite">
					<image :src="mainData.collectMe&&mainData.collectMe.length>0&&mainData.collectMe[0].status==1?
					'../../static/images/details-icon7-a.png':'../../static/images/details-icon7.png'" mode=""></image>
					<view>{{mainData.collectMe?mainData.collect.length:''}}人</view>
				</view>
				<view class="ite" @click="callPhone">
					<image src="../../static/images/details-icon8.png" mode=""></image>
					<view>客服</view>
				</view>
				<view class="ite pr" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
					<view class="car-num">{{count}}</view>
					<image src="../../static/images/details-icon9.png" mode=""></image>
					<view>购物车</view>
				</view>
			</view>
			<view class="flexCenter" style="width: 38%;">
				<view class="numBox flex">
					<view @click="counter('-')">-</view>
					<view class="num pubColor">{{count}}</view>
					<view class="pubBj" style="color:  white;" @click="counter('+')">+</view>
				</view>
			</view>
		</view>
		<!-- 底部菜单按钮 end -->
		
		<view class="specsShow whiteBj pdlr4" v-if="is_combineShow">
			<view class="closebtn fs18 color6 pdr10 pdl10 mgt5" @click="combineShow">×</view>
			<view class="pdt15 pdb15 center borderB1">选择促销</view>
			<view class="editLine fs13">
				<view class="item pdtb10 borderB1" @click="Router.navigateTo({route:{path:'/pages/promotion/promotion?id='+mainData.id}})">
					<view class="flex">
						<view style="width: auto;">特价</view>
						<view class="color6" style="margin-left: 20rpx;">{{mainData.title}} {{mainData.combine_price}}/{{mainData.combine_count}}瓶</view>
					</view>
				
					<view class="flexRowBetween">
						<view class="price">{{mainData.combine_price}}</view>
						<view class="numBox flex">
							
							<view class="pubBj" style="color:  white;" >+</view>
						</view>
					</view>
					
				</view>
				<view class="item pdtb10 borderB1">
					<view>
						<view class="ll color6">不参与促销</view>
					</view>
					
					<view class="flexRowBetween">
						<view class="price">{{mainData.price}}</view>
						<view class="numBox flex">
							<view @click="counter('-')">-</view>
							<view class="num pubColor">{{count}}</view>
							<view class="pubBj" style="color:  white;" @click="counter('+')">+</view>
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
				is_show:false,
				curr:1,
				is_couponShow:false,
				is_specsShow:false,
				count:0,
				is_carNumShow:false,
				mainData:{},
				couponData:[],
				totalMessage:0,
				stars: [0, 1, 2, 3, 4],
				normalSrc: '../../static/images/details-icon2.png',
				selectedSrc: '../../static/images/details-icon1.png',
				halfSrc: '../../static/images/details-icon1-a.png',
				messageData:[],
				cartData:[],
				goodRate:0,
				is_combineShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getCouponData','getLabelData'], self);
		},
		
		/* onShow(){
			const self = this;
			self.cartData = self.$Utils.getStorageArray('cartData');
			console.log('self.cartData',self.cartData)
		}, */
		
		methods: {
			
			collect(){
				const self = this;
				if(self.mainData.collectMe.length>0){
					self.logUpdate()
				}else{
					self.logAdd()
				}
			},
			
			logUpdate() {
				const self = this;
				
				const postData = {
					tokenFuncName: 'getProjectToken',
					searchItem:{
						id:self.mainData.collectMe[0].id
					}
				};
				postData.data = {
				};
				if(self.mainData.collectMe[0].status==1){
					postData.data.status=-1
				}else{
					postData.data.status=1
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						if(self.mainData.collectMe[0].status==1){
							self.$Utils.showToast('取消成功', 'none');
						}else{
							self.$Utils.showToast('收藏成功', 'none');
						}
						setTimeout(function() {
							self.getMainData()
						}, 1000);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.logUpdate(postData, callback);
			},
			
			logAdd() {
				const self = this;
				
				const postData = {
					tokenFuncName: 'getProjectToken',
				};
				postData.data = {
					thirdapp_id:2,
					relation_id:self.mainData.id,
					relation_table:'product',
					type:2,
					user_no:uni.getStorageSync('user_info').user_no,
					
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.$Utils.showToast('收藏成功', 'none');
						setTimeout(function() {
							self.getMainData()
						}, 1000);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.logAdd(postData, callback);
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
			
			submit(index){
				const self = this;
				self.orderList = [
					{coupon_id:self.couponData[index].id,count:1,type:self.couponData[index].type}
				];
				self.couponAdd()
			},
			
			couponAdd() {
				const self = this;
				var now =  (new Date()).getTime();
				const postData = {
					tokenFuncName: 'getProjectToken',
				};
				postData.couponList = self.$Utils.cloneForm(self.orderList);
				postData.pay = {
					score: 0
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.$Utils.showToast('领取成功', 'none')
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.couponAdd(postData, callback);
			},
			
			getCouponData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData.push.apply(self.couponData, res.info.data);
					}
					console.log('self.couponData', self.couponData)
					self.$Utils.finishFunc('getCouponData');
				};
				self.$apis.couponGet(postData, callback);
			},
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			
			couponShow(){
				const self = this;
				self.is_show =!self.is_show;
				self.is_couponShow =!self.is_couponShow
			},
			specsShow(){
				const self = this;
				self.is_show =!self.is_show;
				self.is_specsShow =!self.is_specsShow
			},
			combineShow(){
				const self = this;
				self.is_show =!self.is_show;
				self.is_combineShow =!self.is_combineShow
			},
			counter(type) {
				const self = this;		
				
				if (type == '+') {
					self.mainData.count++;
					self.count++
					console.log(232)
					self.is_carNumShow =true;
				} else {
					if (self.mainData.count > 1) {
						self.mainData.count--;
						self.count--
						
					}
				};
				
				self.$Utils.setStorageArray('cartData',self.mainData,['id','behavior'],999);
				console.log('223',self.mainData.count)
				
			},
			
			carNumShow(){
				const self = this;
				if(self.mainData.combine_price!=''){
					self.combineShow()
					return
				};
				self.is_carNumShow =!self.is_carNumShow;
				self.mainData.isSelect = true;
				self.mainData.count = 1;
				self.mainData.behavior = 0;
				self.count = 1;
				self.$Utils.setStorageArray('cartData',self.mainData,['id','behavior'],999);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				postData.getAfter = {
					combineProduct:{
						tableName:'Product',
						middleKey:'combine_no',
						key:'product_no',
						searchItem:{
							status:1,
						},
						condition:'='
					},
					relationProduct:{
						tableName:'Product',
						middleKey:'category_id',
						key:'category_id',
						searchItem:{
							status:1,
							id:['not in',self.id]
						},
						condition:'='
					},
					collect:{
						
						tableName:'Log',
						middleKey:'id',
						key:'relation_id',
						searchItem:{
							status:1,
							type:2,
							relation_table:'product',
							user_type:0
						},
						condition:'='
					},
					collectMe:{
						
						tableName:'Log',
						middleKey:'id',
						key:'relation_id',
						searchItem:{
							status:1,
							type:2,
							relation_table:'product',
							user_no:uni.getStorageSync('user_info').user_no,
							status:['in',[1,-1]],
						
						},
						condition:'='
					}
				};
				 const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.mainData.isSelect = true;
						self.mainData.count = 0;
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.getMessageData();
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getMessageData(isNew) {
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					//relation_id :self.id,
					type:1,
					relation_table:'Product'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.goodMessage = [];
						self.messageData.push.apply(self.messageData, res.info.data);
						for (var i = 0; i < self.messageData.length; i++) {
							if(self.messageData[i].score>=8){
								self.goodMessage.push(self.messageData[i])
							}
						}
					}
					self.goodRate = (self.goodMessage.length/self.messageData.length)*100;
					console.log('23',res.info.total)
					self.totalMessage = res.info.total;
					console.log('self.messageData', self.messageData)
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/NavTab.css";
	@import "../../assets/style/star.css";
	@import "../../assets/style/proList.css";
	@import "../../assets/style/proDetail.css";
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/couponlist.css";
	page{padding-bottom: 100rpx;}
	.orderNav .tt{width: 33.3%;}
	
	.swiper-box{height: 500rpx;box-sizing: border-box; width: 100%;border-radius: 8rpx; overflow: hidden;}
	.swiper-box image {width: 100%;height: 100%;}
	
	/* 规格弹框内容 */
	.specsShow{width: 100%;padding: 0 4%;box-sizing: border-box;position: fixed; bottom: 0;left: 0;right: 0;z-index: 42;}
	.specsShow .editLine{max-height: 720rpx;overflow-y: auto;}
	
	.car-num{display:inline-block;padding: 0 6rpx;line-height: 26rpx;font-size: 20rpx;color: #fff;border-radius:13rpx;background: #FF2121;position: absolute;top: -10rpx;left: 80rpx;z-index: 2;}
</style>

