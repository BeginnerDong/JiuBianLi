<template>
	<view>
		
		<view class="pdlr4 pdt15">
			<view class="proList proList-row">
				<view class="item flexRowBetween" v-for="(item,index) in proListDate" :key="index">
					<view class="item_selBtn flexCenter">
						<image class="seltIcon" src="../../static/images/shopping-icon3.png" mode=""></image>
					</view>
					<view class="R_cont flexRowBetween">
						<view class="pic">
							<image src="../../static/images/home-img10.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
							
							<view class="flexRowBetween B-price">
								<view class="price fs14">{{item.price}}</view>
								<view class="numBox flex">
									<view @click="counter(index,'-')">-</view>
									<view class="num">{{item.count}}</view>
									<view @click="counter(index,'+')">+</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="flex iconTit pubColor pdb10 fs13">
				<image class="mgr5" style="width: 37rpx;height: 33rpx;" src="../../static/images/shopping-icon4.png" mode=""></image>
				<view>您有可领取的福利，选中后随正常订单配送</view>
			</view>
			<view class="proList proList-row">
				<view class="item flexRowBetween"  v-for="(item,index) in proListDate" :key="index">
					<view class="item_selBtn flexCenter">
						<image class="seltIcon" src="../../static/images/shopping-icon2.png" mode=""></image>
					</view>
					<view class="R_cont flexRowBetween">
						<view class="pic">
							<image src="../../static/images/home-img10.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
							
							<view class="flexRowBetween B-price">
								<view class="price fs14">56.00</view>
								<view class="flex">×1</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		
			<view class="flex iconTit pubColor pdb10 pdt5 fs13">
				<view class="pubBj radius8 fs11 white pdl5 pdr5 mgr5">换购</view>
				<view>您有符合的满额促销，请选择(可选择)</view>
			</view>
			<view class="fs13 ftw mgb10">已满49元，可换购以下商品</view>
			<view class="proList proList-row">
				<view class="item flexRowBetween" v-for="(item,index) in huangouDate" :key="index">
					<view class="item_selBtn flexCenter">
						<image class="seltIcon" src="../../static/images/shopping-icon3.png" mode=""></image>
					</view>
					<view class="R_cont flexRowBetween">
						<view class="pic">
							<image src="../../static/images/home-img10.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
							
							<view class="flexRowBetween B-price">
								<view class="price fs14">{{item.price}}</view>
								<view class="numBox flex">
									<view @click="counter(index,'-')">-</view>
									<view class="num">{{item.count}}</view>
									<view @click="counter(index,'+')">+</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 全选 -->
		<view class="xqbotomBar flexRowBetween borderB1"  style="bottom: 110rpx;">
			<view class="left flexRowBetween"  style="width: 66%;">
				<view class="flex color6">
					<image class="seltIcon mgr5" src="../../static/images/shopping-icon2.png" mode=""></image>
					<view>全选</view>
					<view class="color6 fs12 mgl10" style="line-height: 36rpx;padding: 0 10rpx;background: #d1d1d1;border-radius: 6rpx;">删除</view>
				</view>
				<view class="fs15 mgr5 flexEnd price">66.00</view>
			</view>
			<view class="payBtn fs14 white" style="width: 34%;"  @click="Router.navigateTo({route:{path:'/pages/orderConfirm/orderConfirm'}})">结算(4)</view>
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
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">分类</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">购物车</view>
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
				count:1,
				proListDate:[
					{price:'59.00',count:'1'},
					{price:'59.00',count:'1'}
				],
				huangouDate:[
					{price:'59.00',count:'1'},
					{price:'59.00',count:'1'}
				]
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			counter(index,type) {
				const self = this;
				if (type == '+') {
					self.proListDate[index].count++;
				} else {
					if (self.proListDate[index].count > 1) {
						self.proListDate[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.proListDate[index], 'id', 999);
				self.countTotalPrice();
			},
			counter(index,type) {
				const self = this;
				if (type == '+') {
					self.huangouDate[index].count++;
				} else {
					if (self.huangouDate[index].count > 1) {
						self.huangouDate[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.huangouDate[index], 'id', 999);
				self.countTotalPrice();
			},
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
	@import "../../assets/style/proList.css";
	@import "../../assets/style/proDetail.css";
	page{padding-bottom:230rpx;background: #F5F5F5;}
	.proList-row .item{align-items: center;}
	.proList-row .item .pic{width: 200rpx;height: 200rpx;margin: 20rpx 0;}
	.proList-row .item .infor{width: 65%;padding: 15px 10px 15px 0;}
	.proList-row .R_cont{width: 90%; align-items: flex-start;}
	
	.item_selBtn{width: 10%;}
	.seltIcon{width: 40rpx; height: 40rpx; display: block;}
	
	/* .iconTit .icon{width: ;} */
</style>

