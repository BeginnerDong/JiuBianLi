<template>
	<view>
		
		<view class="pdlr4 whiteBj">
			<view class="pdtb15 flexRowBetween">
				<view class="">
					<view>营业时间</view>
					<view class="fs14 ftw">10:00~22:30</view>
				</view>
				<view class="seltSwitch fs12 flexCenter center">
					<view class="child" :class="curr==1?'on':''" @click="changeCurr('1')">立即送达</view>
					<view class="child" :class="curr==2?'on':''" @click="changeCurr('2')">预约配送</view>
				</view>
			</view>
		</view>
		
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
					<view class="item flexRowBetween"  v-for="(item,index) in proListDate" :key="index">
						<view class="pic">
							<image src="../../static/images/home-img10.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
							<view class="flexRowBetween B-price">
								<view class="price fs14">56.00</view>
								<view class="flex">×3</view>
							</view>
						</view>
					</view>
				</view>
				<view class="myRowBetween pdlr4">
					<view class="item flexRowBetween">
						<view class="ll fs13 ftw">红包</view>
						<view class="rr fs12">
							<view class="color9">暂无红包可用</view>
							<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
						</view>
					</view>
					<view class="item flexRowBetween">
						<view class="ll fs13 ftw">优惠券</view>
						<view class="rr fs12" @click="Router.navigateTo({route:{path:'/pages/order-useCoupon/order-useCoupon'}})">
							<view class="color9 pubColor">15元电商优惠券</view>
							<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
						</view>
					</view>
					<view class="item flexRowBetween">
						<view class="ll fs13 ftw"></view>
						<view class="rr fs12 color6">
							合计<view class="price ftw fs15 mgl5">56.00</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="myRowBetween pdlr4 whiteBj radius10  mgb15">
				<view class="item flexRowBetween" style="border-top: 0;">
					<view class="ll fs13 ftw">温度</view>
					<view class="rr fs12 color9" @click="tmptShow">
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll fs13 ftw">订单备注</view>
					<view class="rr fs12 color9" @click="Router.navigateTo({route:{path:'/pages/orderConfirm-remarks/orderConfirm-remarks'}})">
						<view>请填写备注内容</view>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll fs13 ftw">发票信息</view>
					<view class="rr fs12 color9" @click="Router.navigateTo({route:{path:'/pages/orderConfirm-invoice/orderConfirm-invoice'}})">
						<view>不开发票</view>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween">
			<view class="left flex mgl5">
				<view class="fs16 price mgr5">856.00</view>
				<view class="fs10 color9">已优惠￥110.00</view>
			</view>
			<view class="payBtn fs16 white" @click="Router.navigateTo({route:{path:'/pages/orderConfirm-pay/orderConfirm-pay'}})">确认下单</view>
		</view>
		<!-- 底部菜单按钮 end -->
		
		<!-- 请选择温度 -->
		<view class="showSel" v-show="is_tmptShow">
			<view class="showSelCont fix">
				<view class="flexRowBetween pdtb15 center borderB1">
					<view class="color6 fs12" @click="tmptShow">取消</view>
					<view>请选择温度</view>
					<view class="color6 fs12">确定</view>
				</view>
				<view class="pdtb15"style="height:300rpx;">
					<view class="selt_specs color6 pdb10 flex fs13">
						<view class="item" :class="specsNum==index?'on':''" @click="changeSpecs(index)" v-for="(item,index) in specsDate" :key="index" >
							<view class="item-tit">{{item}}</view>
						</view>
					</view>
					<view class="fs13 color9">仅限啤酒和饮料选择温度</view>
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
				curr:1,
				proListDate:[{},{}],
				is_tmptShow:false,
				specsNum:0,
				specsDate:['冰镇','常温','半冰半常温','冰镇']
			}
		},
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr=curr
				}
			},
			tmptShow(){
				const self = this;
				self.is_tmptShow = !self.is_tmptShow
			},
			changeSpecs(index){
				const self = this;
				self.specsNum = index
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
	@import "../../assets/style/proList.css";
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/proDetail.css";
	page{background: #F5F5F5;padding-bottom: 110rpx;}
	.seltSwitch{width: 320rpx;height: 60rpx;line-height: 60rpx;background: #ededed;border-radius: 30rpx;}
	.seltSwitch .child{width: 50%;}
	.seltSwitch .child.on{background: #FF2121;color: #fff;border-radius: 30rpx;}

	.proList-row .item{margin-bottom: 0;}
	.proList-row .item .pic{width: 160rpx; height: 160rpx;padding: 20rpx;}
	.proList-row .item .infor{height: 200rpx; width: 70%;}
	
	.myRowBetween .item{padding: 30rpx 0;border-top: 1px solid #eee;}
	
	/* 温度弹框 */
	.showSel{width: 100%;height: 100%;background: rgba(0, 0, 0, 0.5);position: fixed;top: 0;right: 0;bottom: 0;left: 0;z-index: 10000;}
	.showSelCont {padding:0 4%;background: #fff;position: fixed;bottom: 0;left: 0;box-sizing: border-box;width: 100%;border-radius: 20rpx 20rpx 0 0;}
	.selt_specs{flex-wrap: wrap;}
	.selt_specs .item{padding: 10rpx 30rpx;border-radius:10rpx;background: #F5F5F5;margin-right: 28rpx;box-sizing: border-box; margin-bottom: 20rpx;}
	.selt_specs .item.on{color: #fff;background: #FF2121;}
</style>

