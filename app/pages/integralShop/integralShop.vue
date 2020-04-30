<template>
	<view>
		
		<view class="intg_head pubBj flexRowBetween">
			<view class="item flexColumn">
				<view class="fs24 ftw tit">{{userInfoData.score}}</view>
				<view class="icon fs13 flexCenter">积分</view>
			</view>
			<view class="item flexColumn" @click="Router.navigateTo({route:{path:'/pages/integralMXi/integralMXi'}})">
				<view class="fs13 tit">积分明细</view>
				<view class="icon flexCenter"><image src="../../static/images/integral-icon.png" mode=""></view>
			</view>
			<view class="item flexColumn"  @click="Router.navigateTo({route:{path:'/pages/integralRecord/integralRecord'}})">
				<view class="fs13 tit">兑换记录</view>
				<view class="icon flexCenter"><image src="../../static/images/integral-icon1.png" mode=""></image></view>
			</view>
		</view>
				
		<view class="pdlr4 pdt15">
			<view class="fs15 pdb10 ftw">商品</view>
			<view class="proList proList-row">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index" >
					<view class="pic" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/integralDetail/integralDetail?id='+$event.currentTarget.dataset.id}})">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">{{item.title}}</view>
						<view class="flexRowBetween B-price fs12 color6">
							<view>单价：{{item.price}}积分</view>
							<view class="jf-num godui" v-if="item.canBuy" 
							@click="goBuy(index)">
								确认兑换
							</view>
							<view class="jf-num" v-else>
								积分不足
							</view>
							
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
				proList:[{},{},{}],
				mainData:[],
				userInfoData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserInfoData'], self);
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
			
			goBuy(index) {
				const self = this;
				console.log(index)
				if (index==undefined) {
					self.$Utils.showToast('商品错误', 'none', 1000);
					return;
				};
				
				var orderList = [
					{product_id:self.mainData[index].id,count:1,
					type:self.mainData[index].type,product:self.mainData[index]},
				];
				uni.setStorageSync('payPro', orderList);
				self.$Router.navigateTo({
					route: {
						path: '/pages/integral_orderConfirm/integral_orderConfirm'
					}
				})
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					}
					console.log('self.userInfoData', self.userInfoData)
					self.getMainData()
				};
				self.$apis.userInfoGet(postData, callback);
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
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['积分商品']],
						},
						middleKey: 'category_id',
						key: 'id',
						condition: 'in',
					},
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							if(parseFloat(self.mainData[i].price)>parseFloat(self.userInfoData.score)){
								self.mainData[i].canBuy = false
							}else{
								self.mainData[i].canBuy = true
							}
						}
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	
	@import "../../assets/style/proList.css";
	page{background: #F5F5F5;}
	.proList-row .item .pic{width: 200rpx; height: 200rpx;}
	.proList-row .item .infor{height: 200rpx;width: 70%;}
	
	.intg_head{padding: 30rpx 4%; color: #F5F5F5;}
	.intg_head .item{width: 33.3%;}
	.intg_head .item .tit{padding-bottom: 20rpx; line-height: 60rpx;}
	.intg_head .item .icon{line-height: 56rpx;}
	.intg_head .item .icon image{width: 56rpx; height: 56rpx;}
	
	.jf-num{border: 1px solid #666;padding:0 10rpx;border-radius:8rpx;position: relative; z-index: 11;}
	.jf-num.godui{border-color:#ff2121; color: #ff2121;}
</style>

