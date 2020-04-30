<template>
	<view>
		
		<view class="mglr4 pdtb15">
			<view>
				<textarea value="" v-model="submitData.passage1" placeholder="请填写备注"  placeholder-class="placeholder"/>
			</view>
			<view class="flexEnd fs12 mgt5 color6">0/50个字</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 140rpx;">
			<button class="btn" type="button"  @click="submit()">确定</button>
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
				submitData:{
					passage1:''
				}
				
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					uni.setStorageSync('passage1',self.submitData.passage1)
					uni.navigateBack({
						delta:1
					})
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
		},
		
	};
</script>

<style>
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	textarea{background: #fff;border-radius: 10rpx; height: 360rpx;}
</style>

