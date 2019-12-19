<template>
	<view>
		<view class="orderNav flex fs13 borderB1">
			<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">商品</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">评论</view>
			<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">详情</view>
		</view>
		
		<!-- 商品 -->
		<view class="" v-show="curr==1">
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
							<view class="item">满50减5</view>
							<view class="item">满100减10</view>
							<view class="item">满100减10</view>
						</view>
					</view>
					<view><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				
				<view class="pdt15 pdb15 flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/promotion/promotion'}})">
					<view class="flex">
						<view class="ftw mgr15">促销</view>
						<view class="flex fs12 C-canshu">
							<view class="cux-Labe mgr10">组合购</view>
							<view class="avoidOverflow" style="width: 400rpx;">50°汾阳青花10 500ml 98/2瓶</view>
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
						<view class="avoidOverflow">品牌、产地产区、规格、酒精度、净含量规格规格</view>
					</view>
				</view>
				<view><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
			<view class="f5H5"></view>
			<view class="mglr4 pingjia">
				<view class="flexRowBetween pdt15 pdb15">
					<view class="flex">
						<view class="ftw mgr15">评价</view>
						<view class="fs12 color6">540人</view>
					</view>
					<view class="color6 fs12 flexEnd">好评度99.4%<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				<view class="flexRowBetween pdb15">
					<view class="flex">
						<image class="us-photo mgr10" src="../../static/images/details-img1.png" mode=""></image>
						<view class="color6">
							<view class="fs13">快乐的猫</view>
							<view class="fs10">2019.11.29</view>
						</view>
					</view>
					<view class="xing flexEnd">
						<view class="starBox mgr5">
							<image src="../../static/images/details-icon1.png" mode=""></image>
							<image src="../../static/images/details-icon1.png" mode=""></image>
							<image src="../../static/images/details-icon1.png" mode=""></image>
							<image src="../../static/images/details-icon1.png" mode=""></image>
							<image src="../../static/images/details-icon2.png" mode=""></image>
						</view>
						<view class="fs12 color9">8分</view>
					</view>
				</view>
				<view class="fs13">好的浓香型白酒用语是,酒体清亮透明,入口绵甜,落口爽净,回味悠长,值得信赖。</view>
				<view class="flexCenter">
					<view class="moreBtn mgt20 mgb20 fs12">更多评价</view>
				</view>
			</view>
			<view class="f5H5"></view>
			<view class="mglr4 pdt15 pdb15">
				<view class="ftw mgr15 pdb10">温馨提示</view>
				<view class="xqInfor fs12">
					<view>非酒水包装等问题，酒水一旦销售将不与退换</view>
					<view>避免光照，通风换将存放</view>
					<view>由于部分商品包装更换较为频繁，请您已收到的商品实物为准，图片近供参考</view>
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
			</view>
		</view>
		<!-- 商品 end -->
		
		<!-- 评论 -->
		<view class="" v-show="curr==2">
			<view class="pingjia">
				<view class="flexRowBetween pdt15 mglr4">
					<view class="flex">
						<view class="ftw mgr15">评价</view>
						<view class="fs12 color6">540人</view>
					</view>
					<view class="color6 fs12 flexEnd">好评度99.4%<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				<view class="pjList" v-for="(item,index) in pingjiaList" :key="index">
					<view class="flexRowBetween pdb15">
						<view class="flex">
							<image class="us-photo mgr10" src="../../static/images/details-img1.png" mode=""></image>
							<view class="color6">
								<view class="fs13">快乐的猫</view>
								<view class="fs10">2019.11.29</view>
							</view>
						</view>
						<view class="xing flexEnd">
							<view class="starBox mgr5">
								<image src="../../static/images/details-icon1.png" mode=""></image>
								<image src="../../static/images/details-icon1.png" mode=""></image>
								<image src="../../static/images/details-icon1.png" mode=""></image>
								<image src="../../static/images/details-icon1.png" mode=""></image>
								<image src="../../static/images/details-icon2.png" mode=""></image>
							</view>
							<view class="fs12 color9">8分</view>
						</view>
					</view>
					<view class="fs13">好的浓香型白酒用语是,酒体清亮透明,入口绵甜,落口爽净,回味悠长,值得信赖。</view>
				</view>
			</view>
		</view>
		
		<!-- 详情 -->
		<view class="pdt5" v-show="curr==3">
			<view class="content ql-editor" style="padding:0;line-height: 0;" v-html="mainData.content">
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		
		<!-- 优惠券弹框 -->
		<view class="couponShow f5bj pdlr4" v-show="is_couponShow">
			<view class="closebtn fs18 color6 pdr10 pdl10 mgt5" @click="couponShow">×</view>
			<view class="pdt15 pdb15 center">可领优惠券</view>
			<view class="couponlist list">
				<view class="item flexRowBetween" v-for="(item,index) in couponShowData" :key="index">
					<view class="flex ll">
						<view class="mny">5</view>
						<view class="infor fs12">
							<view>5元优惠券(香烟不可用)</view>
							<view class="flex mgt15"><view class="labBtn">满50元可用</view></view>
						</view>
					</view>
					<view class="rr flexEnd">
						<view class="lq-btn">领取</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 规格弹框 -->
		<view class="specsShow whiteBj pdlr4" v-show="is_specsShow">
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
		<view class="xqbotomBar flexRowBetween" v-show="!is_carNumShow">
			<view class="left flexRowBetween">
				<view class="ite" @click="Router.navigateTo({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/details-icon7.png" mode=""></image>
					<view>75人</view>
				</view>
				<view class="ite">
					<image src="../../static/images/details-icon8.png" mode=""></image>
					<view>客服</view>
				</view>
				<view class="ite">
					<image src="../../static/images/details-icon9.png" mode=""></image>
					<view>购物车</view>
				</view>
			</view>
			<view class="payBtn fs16 white" @click="carNumShow">加入购物车</view>
		</view>
		<!-- 底部菜单按钮 end -->
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween" v-show="is_carNumShow">
			<view class="left flexRowBetween">
				<view class="ite">
					<image src="../../static/images/details-icon7.png" mode=""></image>
					<view>75人</view>
				</view>
				<view class="ite">
					<image src="../../static/images/details-icon8.png" mode=""></image>
					<view>客服</view>
				</view>
				<view class="ite pr" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
					<view class="car-num">1</view>
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
				curr:1,
				labelData: [
					"../../static/images/details-img.png",
					"../../static/images/details-img.png",
					"../../static/images/details-img.png",
				],
				proList:[{},{},{},{}],
				pingjiaList:[{},{},{},{},{},{},{}],
				couponShowData:[{},{},{},{}],
				is_couponShow:false,
				is_specsShow:false,
				count:1,
				is_carNumShow:false,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
			},
			carNumShow(){
				const self = this;
				self.is_carNumShow =!self.is_carNumShow;
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
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

