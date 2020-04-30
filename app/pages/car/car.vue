<template>
	<view>

		<view class="pdlr4 pdt15">
			<view class="proList proList-row">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="item_selBtn flexCenter" @click="choose(index)">
						<image class="seltIcon" :src="item.isSelect?'../../static/images/shopping-icon3.png':'../../static/images/shopping-icon2.png'"
						 mode=""></image>
						
					</view>
					<view class="R_cont flexRowBetween">
						<view class="pic">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow">{{item.title}}</view>

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

			<view class="flex iconTit pubColor pdb10 fs13" v-if="memberData.length>0&&!hasBuyMember">
				<image class="mgr5" style="width: 37rpx;height: 33rpx;" src="../../static/images/shopping-icon4.png" mode=""></image>
				<view>您有可领取的福利，选中后随正常订单配送</view>
			</view>
			<view class="proList proList-row" v-if="memberData.length>0&&!hasBuyMember">
				<view class="item flexRowBetween" v-for="(item,index) in memberData" :key="index">
					<view class="item_selBtn flexCenter" @click="chooseMember(index)">
						<image class="seltIcon" :src="item.isSelect?'../../static/images/shopping-icon3.png':'../../static/images/shopping-icon2.png'"
						 mode="">

						</image>
					</view>
					<view class="R_cont flexRowBetween">
						<view class="pic">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow">{{item.title}}</view>

							<view class="flexRowBetween B-price">
								<view class="price fs14">{{item.price}}</view>
								<view class="flex">×{{item.count}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>

			<view class="flex iconTit pubColor pdb10 pdt5 fs13">
				<view class="pubBj radius8 fs11 white pdl5 pdr5 mgr5">换购</view>
				<view>您有符合的满额促销，请选择(可选择)</view>
			</view>
			<view class="fs13 ftw mgb10">满{{quota}}元，可换购以下商品</view>
			<view class="proList proList-row">
				<view class="item flexRowBetween" v-for="(item,index) in addData" :key="index">
					<view class="item_selBtn flexCenter" @click="chooseAdd(index)">
						<image class="seltIcon" :src="item.isSelect?'../../static/images/shopping-icon3.png':'../../static/images/shopping-icon2.png'"
						 mode=""></image>
					</view>
					<view class="R_cont flexRowBetween">
						<view class="pic">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow">{{item.title}}</view>

							<view class="flexRowBetween B-price">
								<view class="price fs14">{{item.price}}</view>
								<view class="numBox flex">
									<view @click="counterAdd(index,'-')">-</view>
									<view class="num">{{item.count}}</view>
									<view @click="counterAdd(index,'+')">+</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 全选 -->
		<view class="xqbotomBar flexRowBetween borderB1" style="bottom: 110rpx;">
			<view class="left flexRowBetween" style="width: 66%;">
				<view class="flex color6">
					<image class="seltIcon mgr5" :src="isChooseAll?'../../static/images/shopping-icon3.png':'../../static/images/shopping-icon2.png'"
					 mode=""></image>
					<view @click="chooseAll">全选</view>
					<view @click="deleteChoose" class="color6 fs12 mgl10" style="line-height: 36rpx;padding: 0 10rpx;background: #d1d1d1;border-radius: 6rpx;">删除</view>
				</view>
				<view class="fs15 mgr5 flexEnd price">{{totalPrice}}</view>
			</view>
			<view class="payBtn fs14 white" style="width: 34%;" 
			@click="goBuy()">结算({{totalCount}})</view>
		</view>

		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/classification/classification'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">分类</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
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
				Router: this.$Router,
				showView: false,
				wx_info: {},
				is_show: false,
				count: 1,
				proListDate: [{
						price: '59.00',
						count: '1'
					},
					{
						price: '59.00',
						count: '1'
					}
				],

				memberData: [],
				addData: [],
				mainData: [],
				totalPrice: 0,
				isChooseAll: true,
				quota: 0,
				totalCount:0,
				orderList:[],
				hasBuyMember:false
			}
		},

		onLoad(options) {
			const self = this;
			console.log('self.mainData',self.mainData)
			if(!uni.getStorageSync('user_token')){
				self.Router.redirectTo({route:{path:'/pages/user/user'}});
				return
			};
			self.$Utils.loadAll(['getMemberData', 'getAddData'], self);
		},

		onShow() {
			const self = this;
			/* if(self.$Utils.getStorageArray('orderList')){
				self.$Utils.delStorageArray('orderList');
			}; */
			self.orderList = [];
			self.quota = parseFloat(uni.getStorageSync('user_info').thirdApp.custom_rule.quota);
			self.mainData = self.$Utils.getStorageArray('cartData');
			self.checkChooseAll();
			self.countTotalPrice();
			
		},

		methods: {
			
			deleteChoose() {
				const self = this;
				uni.showModal({
				    title: '提示',
				    content: '确定要删除所选商品吗？',
					
				    success: function (res) {
				        if (res.confirm) {
				            for (var i = 0; i < self.mainData.length; i++) {
				            	if (self.mainData[i].isSelect) {
				            		self.$Utils.delStorageArray('cartData', self.mainData[i], 'id');
				            	}
				            };
				            self.mainData = self.$Utils.getStorageArray('cartData');
				            console.log('2323',self.mainData)
				            self.checkChooseAll();
				            self.countTotalPrice();
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});	
			},
			
			goBuy(){
				const self = this;
				
				var newArray = [];
				 newArray = newArray.concat(self.memberData, self.addData,self.mainData);
				console.log('newArray', newArray)
				for (var i = 0; i < newArray.length; i++) {
					if (newArray[i].isSelect) {
						self.orderList.push(
							{product_id:newArray[i].id,count:newArray[i].count,
							type:newArray[i].type,product:newArray[i]},
						);
					};
				};
				if (self.orderList.length == 0) {
					self.$Utils.showToast('未选择商品', 'none', 1000);
					return;
				};
				self.$Utils.setStorageArray('orderList', self.orderList);
				self.Router.redirectTo({route:{path:'/pages/orderConfirm/orderConfirm'}})
			},

			checkChooseAll() {
				const self = this;
				var isChooseAll = true;
				for (var i = 0; i < self.mainData.length; i++) {
					if (!self.mainData[i].isSelect) {
						isChooseAll = false;
					};
				};
				self.isChooseAll = isChooseAll;
				console.log('checkChooseAll',self.mainData)
			},

			chooseAll() {
				const self = this;
				self.isChooseAll = !self.isChooseAll;
				
				console.log(self.isChooseAll)
				for (var i = 0; i < self.mainData.length; i++) {
					self.mainData[i].isSelect = self.isChooseAll;
					self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
				};
				console.log(self.mainData)
				self.countTotalPrice();
			},

			chooseMember(index) {
				const self = this;
				if (self.memberData[index].isSelect) {
					self.memberData[index].isSelect = false;
				} else {
					self.memberData[index].isSelect = true;
				};
				self.countTotalPrice();
			},

			choose(index) {
				const self = this;

				if (self.mainData[index].isSelect) {
					self.mainData[index].isSelect = false;
				} else {
					self.mainData[index].isSelect = true;
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);

				self.checkChooseAll();
				self.countTotalPrice();
			},
			
			chooseAdd(index) {
				const self = this;
				if(parseFloat(self.totalPrice)<self.quota){
					uni.showModal({
						title: '提示',
						content: '未达到换购标准金额',
						showCancel: false
					});
					return
				};
				if (self.addData[index].isSelect) {
					self.addData[index].isSelect = false;
				} else {
					self.addData[index].isSelect = true;
				};
				self.countTotalPrice();
			},


			getMemberData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 4,
					//city_id:uni.getStorageSync('city_id')
				};
				postData.order = {
					listorder: 'desc'
				};
				postData.getAfter = {
					order:{
						token:uni.getStorageSync('user_token'),
						tableName:'Order',
						middleKey:'id',
						key:'product_id',
						searchItem:{
							pay_status:1,
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.memberData.push.apply(self.memberData, res.info.data);
						for (var i = 0; i < self.memberData.length; i++) {
							self.memberData[i].count = 1
							self.memberData[i].isSelect = false;
							if(self.memberData[i].order.length>0){
								self.hasBuyMember = true
							}
						}
					}
					console.log('self.memberData', self.memberData)
					self.$Utils.finishFunc('getMemberData');
				};
				self.$apis.productGet(postData, callback);
			},

			getAddData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 3,
					//city_id:uni.getStorageSync('city_id')
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addData.push.apply(self.addData, res.info.data);
						for (var i = 0; i < self.addData.length; i++) {
							self.addData[i].count = 1
							self.addData[i].isSelect = false
						}
					}
					console.log('self.addData', self.addData)
					self.$Utils.finishFunc('getAddData');
				};
				self.$apis.productGet(postData, callback);
			},

			counter(index, type) {
				const self = this;

				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);

				self.countTotalPrice();
			},
			
			counterAdd(index, type) {
				const self = this;
				if (type == '+') {
					self.addData[index].count++;
				} else {
					if (self.addData[index].count > 1) {
						self.addData[index].count--;
					}
				};
				self.countTotalPrice();
			},
			


			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				self.totalCount = 0;
				var newArray = [];
				 newArray = newArray.concat(self.memberData, self.addData,self.mainData);
				console.log('newArray', newArray)
				for (var i = 0; i < newArray.length; i++) {
					if (newArray[i].isSelect) {
						self.totalPrice += newArray[i].price * newArray[i].count;
						self.totalCount += newArray[i].count
					};
				};
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2);
				console.log('countTotalPrice',self.mainData)
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proList.css";
	@import "../../assets/style/proDetail.css";

	page {
		padding-bottom: 230rpx;
		background: #F5F5F5;
	}

	.proList-row .item {
		align-items: center;
	}

	.proList-row .item .pic {
		width: 200rpx;
		height: 200rpx;
		margin: 20rpx 0;
	}

	.proList-row .item .infor {
		width: 65%;
		padding: 15px 10px 15px 0;
	}

	.proList-row .R_cont {
		width: 90%;
		align-items: flex-start;
	}

	.item_selBtn {
		width: 10%;
	}

	.seltIcon {
		width: 40rpx;
		height: 40rpx;
		display: block;
	}

	/* .iconTit .icon{width: ;} */
</style>
