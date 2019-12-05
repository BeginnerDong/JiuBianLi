<template>
	<view>
		<view style="position: relative;z-index: 90;">
			<!-- 搜索 -->
			<view class="seachbox flexRowBetween whiteBj borderB1" style="padding-bottom: 30rpx;">
				<view class="flex rr" style="width: 85%;">
					<button class="seachBtn" type="button"></button>
					<view class="input">
						<input type="text" name="" value="" placeholder="乌苏 西风 洋河" placeholder-class="placeholder" />
					</view>
					<view class="delt flex"><text>×</text></view>
				</view>
				<view class="Rseach pubColor fs15">搜索</view>
			</view>
			<view class="whiteBj category fs13 color9 flexRowBetween borderB1">
				<view class="item on flexCenter" @click="zongheShow">综合</view>
				<view class="item flexCenter">价格</view>
				<view class="item flexCenter">推荐</view>
				<view class="item flexCenter"  @click="styleShow">
					<image class="icon" style="width: 36rpx; height: 28rpx;" src="../../static/images/classification-icon13.png" mode=""></image>
				</view>
			</view>
			<view class="zongheShow whiteBj" v-show="is_zongheShow">
				<view class="item flexRowBetween" :class="num==1?'on':''" @click="changeNum('1')">
					<view class="tt">综合排序</view>
					<image class="selIcon" src="../../static/images/classification-icon10.png" mode=""></image>
				</view>
				<view class="item flexRowBetween" :class="num==2?'on':''" @click="changeNum('2')">
					<view class="tt">销量排序</view>
					<image class="selIcon" src="../../static/images/classification-icon10.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="black-bj" v-show="is_show" ></view>
		
		<view class="pdlr4 pdt20 pdb15">
			<view class="proList flex" v-show="!is_styleShow">
				<view class="item" v-for="(item,index) in proList" :key="index" @click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail'}})">
					<view class="pic">
						<image src="../../static/images/home-img10.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
						<view class="flex mgb10">
							<view class="lab">组合装</view>
						</view>
						<view class="flexRowBetween">
							<view class="price">56.00</view>
							<view class="adBtn">+</view>
						</view>
					</view>
				</view>
			</view>
		
			<view class="proList proList-row" v-show="is_styleShow">
				<view class="item flexRowBetween" v-for="(item,index) in proList" :key="index" @click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail'}})">
					<view class="pic">
						<image src="../../static/images/home-img10.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
						<view class="flex mgb10">
							<view class="lab">组合装</view>
						</view>
						<view class="flexRowBetween B-price">
							<view class="price">56.00</view>
							<view class="adBtn">+</view>
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
				proList:[{},{},{},{},{},{}],
				is_zongheShow:false,
				num:1,
				is_styleShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			zongheShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_zongheShow = !self.is_zongheShow
			},
			changeNum(num){
				const self = this;
				if(num!=self.num){
					self.num = num
				}
				self.is_show = !self.is_show
				self.is_zongheShow = !self.is_zongheShow
			},
			styleShow(){
				const self = this;
				self.is_styleShow = !self.is_styleShow;
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
	@import "../../assets/style/seach.css";
	@import "../../assets/style/proList.css";
	page{background: #F5F5F5;}
	
	.navdian span{width: 10rpx;height: 10rpx;border-radius: 50%;background: #FF2121;margin: 0 4rpx;}
	.category .item{width: 25%; border-right: 1px solid #eee;}
	.category .item:last-child{border-right:0 ;}
	
	.category{padding: 20rpx 4%;}
	.category .item.on{color: #FF2121;}
	.zongheShow{padding: 20rpx 6%;}
	.zongheShow .item{padding: 20rpx 0; color: #666;}
	.zongheShow .item .selIcon{width: 26rpx; height: 19rpx; display: block; display: none;}
	.zongheShow .item.on{color: #222;font-weight: bold;}
	.zongheShow .item.on .selIcon{display: block;}
	
</style>

