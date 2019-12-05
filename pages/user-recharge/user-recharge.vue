<template>
	<view>
		<view class="center myHead flexColumn whiteBj">
			<view class="mny ftw">0.00</view>
			<view class="flex fs13 mgt10"><image style="width: 28rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/top-up-icon.png" mode=""></image>余额(元)</view>
			<view class="flex fs12 pdtb20" @click="Router.navigateTo({route:{path:'/pages/user-rechargeDetail/user-rechargeDetail'}})"><image style="width: 28rpx; height: 30rpx;margin-right: 10rpx;" src="../../static/images/top-up-icon1.png" mode=""></image>查看明细</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pdlr4 whiteBj">
			<view class="fs14 ftw pdt15">充值金额</view>
			<view class="input-money pdtb10"><input type="number" value="100" placeholder="请输入金额" /></view>
			
			<view class="specsList flexRowBetween center fs16 color6 pdt15 ftw" >
				<view class="item" :class="curr==index?'on':''" @click="changeCurr(index)" v-for="(item,index) in specsList" :key="index">{{item}}</view>
			</view>
		</view>
		
		<view class="b-fixBtn center submitbtn whiteBj">
			<view class="fs12 mgb5">商务部单用途商业预卡企业备案编码：412353001.21</view>
			<button class="btn" type="button">立即充值</button>
			<view class="fs12 mgt5 flexCenter">点击充值即同意<view class="pubColor" @click="userzcShow">《预付卡使用章程》</view>。余额不可提现、退还。</view>
		</view>
		
		<!-- 章程弹框 -->
		<view class="xieyiAlert" v-show="is_userzcShow">
			<view class="infor">
				<view class="center pdtb10">预付卡使用章程</view>
				<view class="cont fs13">
					<view>1、龙口市东海工业园发哈市风行风烘干机打发股东会撒娇立法会估计都撒好几个回访近二手房。</view>
					<view>2、龙口市东海工业园发哈市风行风烘干机打发股东会撒娇立法会估计都撒好几个回访近二手房。</view>
					<view>3、龙口市东海工业园发哈市风行风烘干机打发股东会撒娇立法会估计都撒好几个回访近二手房。</view>
					<view>4、龙口市东海工业园发哈市风行风烘干机打发股东会撒娇立法会估计都撒好几个回访近二手房。</view><view>1、龙口市东海工业园发哈市风行风烘干机打发股东会撒娇立法会估计都撒好几个回访近二手房。</view>
					<view>2、龙口市东海工业园发哈市风行风烘干机打发股东会撒娇立法会估计都撒好几个回访近二手房。</view>
					<view>3、龙口市东海工业园发哈市风行风烘干机打发股东会撒娇立法会估计都撒好几个回访近二手房。</view>
					<view>4、龙口市东海工业园发哈市风行风烘干机打发股东会撒娇立法会估计都撒好几个回访近二手房。</view>
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
				specsList:['100元','200元','500元','800元','1000元','其他金额'],
				curr:0,
				is_userzcShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(index){
				const self = this;
				self.curr = index;
			},
			userzcShow(){
				const self = this;
				self.is_userzcShow = !self.is_userzcShow
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

