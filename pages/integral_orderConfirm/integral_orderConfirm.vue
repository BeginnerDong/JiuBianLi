<template>
	<view>
		
		<view class="mglr4 pdtb10">
			<view class="whiteBj radius10 mgb15">
				<view class="flex"><image class="w" style="" src="../../static/images/the-order-img.png" mode="widthFix"></image></view>
				<view class="pdtb10 mglr4 flexRowBetween fs13 borderB1" v-show="curr==2">
					<view>选择时间</view>
					<view class="flexEnd color6 fs12">
						<view>请选择时间</view>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="flexRowBetween pdtb15 mglr4"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<view class="fs13">
						<view>陕西省西安市雁塔区高新大都荟</view>
						<view class="color6 flex mgt5">
							<view class="mgr10">张思德</view>
							<view class="">15689562352</view>
						</view>
					</view>
					<view class="flexEnd" style="width: 20%;">
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
			</view>
			
			<view class="whiteBj radius10  mgb15">
				<view class="proList proList-row">
					<view class="item" >
						<view class="flexRowBetween">
							<view class="pic">
								<image src="../../static/images/home-img10.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
								<view class="flexRowBetween B-price">
									<view class="price fs14">56.00 <text class="yuanJia">56.00</text></view>
									<view class="flex">×3</view>
								</view>
							</view>
						</view>
						<view class="flexEnd mglr4 pdt10 pdb15 fs12 color6">
							总计 <view class="red mgl10">积分：<text class="fs15">56</text></view>
						</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween">
			<view class="left flex mgl5 fs14 red">积分：<text class="fs16">56</text></view>
			<view class="payBtn fs16 white" @click="Router.navigateTo({route:{path:'/pages//'}})">确认兑换</view>
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
				mainData:[],
			}
		},
		
		methods: {
			
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
			
		}
	};
</script>

<style>
	
	@import "../../assets/style/proList.css";
	@import "../../assets/style/proDetail.css";
	page{background: #F5F5F5;padding-bottom: 110rpx;}
	.proList-row .item{margin-bottom: 0;}
	.proList-row .item .pic{width: 160rpx; height: 160rpx;padding: 20rpx;}
	.proList-row .item .infor{height: 200rpx; width: 70%;}
</style>

