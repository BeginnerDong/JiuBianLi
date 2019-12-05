<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text" name="" value="" placeholder="乌苏 西风 洋河" placeholder-class="placeholder" />
				</view>
				<view class="delt flex"><text>×</text></view>
			</view>
			<view class="Rseach pubColor fs15" @click="Router.navigateTo({route:{path:'/pages/seach-product/seach-product'}})">搜索</view>
		</view>
		<view class="pdlr4 borderB1">
			<view class="fs15 pdt20 pdb15 ftw">热门搜索</view>
			<view class="hotLabel center fs13 flex ">
				<view class="item" v-for="(item,index) in hotLabel" :key="index">{{item}}</view>
			</view>
			
			<view class="fs15 pdt10 pdb15 ftw">历史记录</view>
			<view class="historyDate  center fs13 flex">
				<view class="item" v-for="(item,index) in historyDate" :key="index">{{item}}</view>
			</view>
		</view>
		
		<view class="fx-deltBtn flexCenter color6" @click="popupShow">
			<image class="icon" src="../../static/images/search-icon1.png" mode=""></image>
			<text>清除搜索记录</text>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="popupShow center whiteBj radius10 pdt20" v-show="is_popupShow">
			<view class="tip-title mgb10 fs15">提示</view>
			<view class="fs13 color6 pdb20 borderB1">确定清空搜索记录吗？</view>
			<view class="flex tip-button">
				<view class="item"  @click="popupShow">取消</view>
				<view class="item pubColor">确定</view>
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
				hotLabel:['乌苏','西风','洋河','泸州','五粮液','国窖','茅台','汾酒','奔富','福佳白','青岛','百威','凯撒'],
				historyDate:['茅台','汾酒','青岛','百威','凯撒'],
				is_popupShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			popupShow(){
				const self=this;
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
				
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
	page{padding-bottom: 110rpx;}
	/* 删除弹框 */
	.popupShow{ width: 80%;position: fixed; top: 50%;left: 50%;transform: translate(-50%,-50%); z-index: 50;box-sizing: border-box;}
	.tip-button .item{width: 50%;box-sizing: border-box; line-height: 100rpx;}
	.tip-button .item:first-child{border-right:1px solid #eee;}
</style>

