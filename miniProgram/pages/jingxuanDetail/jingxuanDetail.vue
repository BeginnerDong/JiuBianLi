<template>
	<view>
		
		<view class="imbox">
			<image class="w" style="heigth" :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode="widthFix"></image>
		</view>
		<view class="flexCenter">
			<view class="q-btn center" 
			@click="toDetail">立即抢购</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		
		methods: {
			
			toDetail(){
				const self = this;
				if(self.mainData.url!=''){
					self.Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+self.mainData.url}})
				}else{
					console.log('未设置')
				}
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.labelGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/seach.css";
	page{background: #F5F5F5;}
	.q-btn{width: 400rpx;background: #FF2121;color: #fff;border-radius: 40rpx;line-height: 80rpx; margin: 80rpx auto;}
</style>

