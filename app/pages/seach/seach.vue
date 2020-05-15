<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input v-model="submitData.keywords" 
					type="text"  placeholder="乌苏 西风 洋河" 
					placeholder-class="placeholder" />
				</view>
				<view class="delt flex" v-if="submitData.keywords!=''" @click="deleteText"><text>×</text></view>
			</view>
			<view class="Rseach pubColor fs15" 
			@click="search">搜索</view>
		</view>
		<view class="pdlr4 borderB1">
			<view class="fs15 pdt20 pdb15 ftw">热门搜索</view>
			<view class="hotLabel center fs13 flex ">
				<view class="item" v-for="(item,index) in hotLabel" @click="clickSearch(item)" :key="index">{{item}}</view>
			</view>
			
			<view class="fs15 pdt10 pdb15 ftw">历史记录</view>
			<view class="historyDate  center fs13 flex">
				<view class="item" v-for="(item,index) in historyData" @click="clickSearch(item)" :key="index">{{item}}</view>
			</view>
		</view>
		
		<view class="fx-deltBtn flexCenter color6" v-if="historyData.length>0" @click="deleteHistory()">
			<image class="icon" src="../../static/images/search-icon1.png" mode=""></image>
			<text>清除搜索记录</text>
		</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
				hotLabel:['乌苏','西风','洋河','泸州','五粮液','国窖','茅台','汾酒','奔富','福佳白','青岛','百威','凯撒'],
				historyData:[],
				
				submitData:{
					keywords:''
				}
			}
		},
		
		onShow() {
			const self = this;
			console.log('self.historyData',self.historyData)
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			clickSearch(item){
				const self = this;
				self.submitData.keywords = item;
				self.search()
			},
			
			deleteText(){
				const self = this;
				console.log('222',self.submitData.keywords)
				self.submitData.keywords = ''
			},
			
			deleteHistory(){
				const self = this;
				uni.showModal({
				    title: '提示',
				    content: '确定要删除历史搜索记录吗',
					showCancel:true,
				    success: function (res) {
				        if (res.confirm) {
				             self.historyData = [];
							 uni.removeStorageSync('historyData')
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});	
			},
			
			search(){
				const self = this;
				if(self.submitData.keywords!=''){
					self.Router.navigateTo({route:{path:'/pages/seach-product/seach-product?keywords='+self.submitData.keywords}})
				}else{
					self.$Utils.showToast('请输入关键词搜索', 'none');
				}
				console.log('self.historyData',self.historyData)
				var position = self.historyData.indexOf(self.submitData.keywords);
				if(position>=0){

				}else{
					console.log('self.historyData',self.historyData)
				  self.historyData.push(self.submitData.keywords);
				};	
				uni.setStorageSync('historyData',self.historyData);
				self.submitData.keywords = '';
			},
			
			getMainData() {
				const self = this;
				console.log('852369');
				if(uni.getStorageSync('historyData')){
					self.historyData = uni.getStorageSync('historyData');
				};
				self.$Utils.finishFunc('getMainData');
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

