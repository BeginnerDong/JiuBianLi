<template>
	<view>
		<view class="center myHead flexColumn whiteBj">
			<view class="mny ftw">{{mainData.balance}}</view>
			<view class="flex fs13 mgt10"><image style="width: 28rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/top-up-icon.png" mode=""></image>余额(元)</view>
			<view class="flex fs12 pdtb20" @click="Router.navigateTo({route:{path:'/pages/user-rechargeDetail/user-rechargeDetail'}})"><image style="width: 28rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/top-up-icon1.png" mode=""></image>查看明细</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pdlr4 whiteBj">
			<view class="fs14 ftw pdt15">充值金额</view>
			<view class="input-money pdtb10" v-if="curr==5">
				<input type="number" placeholder="请输入金额" v-model="price"/>
			</view>
			
			<view class="specsList flexRowBetween center fs16 color6 pdt15 ftw" >
				<view class="item" :class="curr==index?'on':''" 
				@click="changeCurr(index)" v-for="(item,index) in specsList" :key="index">{{item}}</view>
				<view class="item" :class="curr==5?'on':''"
				@click="changeCurr(5)"  :key="index">其他金额</view>
			</view>
		</view>
		
		<view class="b-fixBtn center submitbtn whiteBj">
			<view class="fs12 mgb5">商务部单用途商业预卡企业备案编码：412353001.21</view>
			<button class="btn" type="button" @click="addOrder()">立即充值</button>
			<view class="fs12 mgt5 flexCenter">点击充值即同意<view class="pubColor" @click="userzcShow">
			《{{artData.title}}》
			</view>。余额不可提现、退还。</view>
		</view>
		
		<!-- 章程弹框 -->
		<view class="xieyiAlert" v-show="is_userzcShow">
			<view class="infor">
				<view class="center pdtb10">{{artData.title}}</view>
				<view class="cont fs13">
					<view class="content ql-editor" style="padding: 0;" v-html="artData.content">
					</view>
				</view>
				<view class="">
					<view class="colseBtn" style="top: auto;bottom: 40rpx;" @click="userzcShow">×</view>
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
				specsList:['100元','200元','500元','800元','1000元'],
				curr:-1,
				is_userzcShow:false,
				price:0,
				orderId:'',
				mainData:{},
				artData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData','getArtData'], self);
		},
		
		methods: {
			
			getArtData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['充值']],
						},
						middleKey: 'menu_id',
						key: 'id',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.artData = res.info.data[0];
			
						const regex = new RegExp('<img', 'gi');
						self.artData.content = self.artData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getArtData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			changeCurr(index){
				const self = this;
				
				if(self.curr!=index){
					self.curr = index;
					if(self.curr<5){
						self.price = parseFloat(self.specsList[self.curr]);
					}else{
						self.price = 0
					}
				};
				
				console.log('self.price',self.price)
			},
			
			userzcShow(){
				const self = this;
				self.is_userzcShow = !self.is_userzcShow
			},
			
			pay() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				const postData = {};	
				postData.wxPay = {
					price:parseFloat(0.01).toFixed(2)
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				postData.payAfter = [
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							type:2,
							count:self.price,
							trade_info:'充值',
							account:1,
							thirdapp_id:2,
							user_no:uni.getStorageSync('user_info').user_no
						},
					},
				];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('支付成功', 'none');
									setTimeout(function() {
										uni.navigateBack({
											delta: 1
										});
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast('支付失败', 'none');
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {						
							self.$Utils.showToast('支付成功', 'none');
							setTimeout(function() {
								uni.navigateBack({
									delta: 1
								});
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none');
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			addOrder() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if(self.orderId!=''){
					self.pay();
					return
				};
				if(self.curr<0){
					self.$Utils.showToast('请选择充值金额', 'none');
					return
				};
				if(self.price<=0){
					self.$Utils.showToast('请输入充值金额', 'none');
					return
				};
				const postData = {};	
				/* postData.wxPay = {
					price:parseFloat(self.submitData.price).toFixed(2)
				}; */
				postData.tokenFuncName = 'getProjectToken',
				postData.data = {
					price:parseFloat(0.01).toFixed(2)
				}
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					}else{
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.addVirtualOrder(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/xieyiAlert.css";
page{background: #F5F5F5;padding-bottom: 240rpx;}	
.myHead .mny{padding-top: 60rpx;font-size: 76rpx;line-height: 76rpx;}
.myRowBetween .item{padding: 20rpx 4%; border-bottom: 1px solid #eee;}

.input-money input{width: 30%;font-size: 30rpx;font-weight: bold;color: #FF2121; height: 60rpx;line-height: 60rpx;border-bottom: 1px solid #FF2121; display: block;}

.specsList{flex-wrap: wrap;}
.specsList .item{width: 210rpx;height: 100rpx; line-height: 100rpx;background: #f1f1f1;border-radius: 10rpx; margin-bottom: 30rpx;}
.specsList .item.on{color: #fff; background: #FF2121;}
.specsList .item:last-child{font-weight: normal;}

.b-fixBtn{padding: 30rpx 0 20rpx 0; position: fixed;bottom: 0;left: 0;right: 0;width: 100%;box-sizing: border-box;}

</style>

