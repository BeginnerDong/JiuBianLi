<template>
	<view>
		
		<view class="mglr4 pdtb15">
			<textarea v-model="submitData.content" placeholder="请输入您对佐森酒业的建议"  placeholder-class="placeholder"/>
		</view>
		
		<view class="submitbtn" style="margin-top: 140rpx;">
			<button class="btn" type="button" @click="Utils.stopMultiClick(submit)">确定</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					
					content:'',
					type:2
				},
			}
		},
		
		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick', true);
		},
		
		methods: {
			
			
			
				
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.content;
				const pass = self.$Utils.checkComplete(newObject);
				if (pass) {								
						self.messageAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入您的建议', 'none')
					console.log(self.submitData)
				};
			},
			
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				console.log('postData',postData)			
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('反馈成功', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageAdd(postData, callback);
			},
			
		},
	};
</script>

<style>
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	textarea{background: #fff;border-radius: 10rpx; height: 360rpx;}
</style>

