<template>
	<view>
		
		<view class="pdlr4 whiteBj">
			<view class="orderNav flex fs14 pdb10">
				<scroll-view scroll-x style="white-space: nowrap;display: flex;">
					<view class="tt" :class="curr==index?'on':''" @click="changeCurr(index)" v-for="(item,index) in classifyNav" :key="index">{{item}}</view>
				</scroll-view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		
		<view class="interct_idexLis">
			<view class="child"  @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail'}})">
				<view class="flex">
					<view class="photo"><image src="../../static/images/home-select-img.png" mode=""></image></view>
					<view class="font13 name">信远斋</view>
				</view>
				<view class="font14 pdtb15">真材细做，货真价实-信远斋桂花酸梅汤</view>
				<view class="imgbox">
					<view class="lisOne">
						<image src="../../static/images/home-select-img1.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="child"  @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail'}})">
				<view class="flex">
					<view class="photo"><image src="../../static/images/home-select-img.png" mode=""></image></view>
					<view class="font13 name">信远斋</view>
				</view>
				<view class="font14 pdtb15">真材细做，货真价实-信远斋桂花酸梅汤</view>
				<view class="imgbox">
					<view class="lisTwo">
						<image src="../../static/images/home-select-img1.png" mode=""></image>
					</view>
					<view class="lisTwo">
						<image src="../../static/images/stoers-img.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="child"  @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail'}})">
				<view class="flex">
					<view class="photo"><image src="../../static/images/home-select-img.png" mode=""></image></view>
					<view class="font13 name">信远斋</view>
				</view>
				<view class="font14 pdtb15">真材细做，货真价实-信远斋桂花酸梅汤</view>
				<view class="imgbox">
					<view class="lisThree">
						<image src="../../static/images/home-select-img1.png" mode=""></image>
					</view>
					<view class="lisThree">
						<image src="../../static/images/stoers-img.png" mode=""></image>
					</view>
					<view class="lisThree">
						<image src="../../static/images/home-select-img1.png" mode=""></image>
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
				curr:0,
				classifyNav:['甄选','白酒','葡萄酒','啤酒','洋酒','饮料','黄酒']
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(index){
				const self = this;
				self.curr = index
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
	@import "../../assets/style/NavTab.css";
	@import "../../assets/style/seach.css";
	@import "../../assets/style/selected.css";
	
	page{padding-bottom: 30rpx;background: #F5F5F5;}
	.orderNav .tt{width: auto; margin-right:50rpx;display: inline-block;}
	
</style>

