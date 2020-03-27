<template>
	<view>
		<view class="proList proList-row mglr4 mgt15">
			<view class="item">
				<view class="flexRowBetween mglr4 pdtb10">
					<view class="fs12 color9">交易时间：{{mainData.create_time}}</view>
					<view class="fs12 red">已完成</view>
				</view>
				<view class="flexRowBetween" v-for="(item,index) in mainData.child" :key="index">
					<view class="pic">
						<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">{{item.title}}</view>
						
						<view class="flexRowBetween B-price">
							<view class="price fs14">{{item.unit_price}}</view>
							<view class="flex">×{{item.count}}</view>
						</view>
					</view>
				</view>
				<view class="pdlr4">
					<view class="flexEnd pdtb15">共{{mainData.count}}件商品 合计:￥{{mainData.price}}</view>
				</view>
			</view>
		</view>
		
		<view class="mglr4 whiteBj radius8 mgt15 pdlr4">
			<view class="pdtb15 flex borderB1">
				<view class="mgr10 fs13">星级评价</view>
				<view class="starBox mgr5" v-for="item in star">
					<image @click="chooseStar(item)" :src="submitData.score>=item?'../../static/images/evaluation-icon1.png':'../../static/images/evaluation-icon.png'" mode=""></image>
				</view>
			</view>
			<view class="pdtb15 flexRowBetween" style="align-items: flex-start;">
				<view class="fs13  mgt5">填写评价</view>
				<view style="width: 80%;">
					<textarea class="fs12 f5bj radius8" v-model="submitData.description" style="height: 240rpx;"  placeholder="请写下您的宝贵意见" />
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100rpx;">
			<view class="btn" @click="Utils.stopMultiClick(submit)">确定</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				wx_info:{},
				is_show:false,
				star:[2,4,6,8,10],
				mainData:{},
				submitData: {
					title:'',
					mainImg:[],
					score:'',
					description:'',
					relation_table:'Product',
					relation_id:'',
					thirdapp_id:2,
					type:1
				},
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			
			self.submitData.title = uni.getStorageSync('user_info').info.name!=''?uni.getStorageSync('user_info').info.name:uni.getStorageSync('user_info').info.phone;
			self.submitData.mainImg = [{type:'image',url:uni.getStorageSync('user_info').info.mainImg.length>0?uni.getStorageSync('user_info').info.mainImg[0].url:''}]
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			chooseStar(item){
				const self = this;
				self.submitData.score = item
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.relation_id = self.mainData.child[0].id;
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData', self.submitData)
				if (pass) {
					self.messageAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [{
				  tableName:'Order',
				  FuncName:'update',
				  searchItem:{
				    id:self.id
				  },
				  data:{
				    isremark:1,
				  }
				}];
				const callback = (data) => {
			
					if (data.solely_code == 100000) {
			
						self.$Utils.showToast('评价成功', 'none')
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
			
				};
				self.$apis.messageAdd(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/proList.css";
	@import "../../assets/style/star.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	.proList-row .item .pic{width: 160rpx;height: 160rpx;}
	.proList-row .item .infor{height: 160rpx;width: 75%;}
	
	.starBox image{width: 28rpx;height: 28rpx;margin-right: 8rpx;}
</style>
