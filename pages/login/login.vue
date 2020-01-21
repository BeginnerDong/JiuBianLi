<template>
	<view>
		<view><image style="width: 100%;height: 412rpx;" src="../../static/images/the-login-icon.png" mode="widthFix"></image></view>
		
		<view class="switchNav color6 fs13 flex mglr4 pdtb15">
			<view class="item" :class="curr==1?'on':''" @click="changeCurr('1')">登录</view>
			<view class="item" :class="curr==2?'on':''" @click="changeCurr('2')">注册</view>
		</view>
		<view v-show="curr==1">
			<view class="loginEdit">
				<view class="item flex">
					<view class="ll">
						<image class="icon" src="../../static/images/the-login-icon2.png" mode=""></image>
					</view>
					<view class="rr">
						<input type="number" v-model="submitData.login_name" placeholder="输入手机号码" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">
						<image class="icon" src="../../static/images/the-login-icon1.png" mode=""></image>
					</view>
					<view class="rr flexRowBetween">
						<view class="" style="width: 60%">
							<input type="text" value="" placeholder="输入验证码" placeholder-class="placeholder" />
						</view>
						<view class="yzm">获取验证码</view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn">
				<button class="btn" type="button" @click="submit">登录</button>
			</view>
		</view>
		
		<view v-show="curr==2">
			<view class="loginEdit">
				<view class="item flex">
					<view class="ll">
						<image class="icon" src="../../static/images/the-login-icon2.png" mode=""></image>
					</view>
					<view class="rr">
						<input type="number" v-model="registerData.phone" placeholder="输入手机号码" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">
						<image class="icon" src="../../static/images/the-login-icon1.png" mode=""></image>
					</view>
					<view class="rr flexRowBetween">
						<view class="" style="width: 60%">
							<input type="text" value="" placeholder="输入验证码" placeholder-class="placeholder" />
						</view>
						<view class="yzm">获取验证码</view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn">
				<button class="btn" type="button" @click="register">注册</button>
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
				registerData:{
					phone:''
				},
				submitData:{
					login_name:''
				}
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
					self.curr = curr
				}
			},
			
			submit() {
				const self = this;
			
				const postData = {
					login_name: self.submitData.login_name,
					
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							
							uni.login({
								success(data) {
									const c_postData = {
										code: data.code,
										token:res.token
									};
									const c_callback = (c_res) => {
										if (c_res.solely_code == 100000) {
											uni.setStorageSync('user_token', res.token);
											uni.setStorageSync('user_info', res.info);
											uni.navigateBack({
												delta:1
											})
										} else {
											self.$Utils.showToast(c_res.msg,'none')
										}
									}
									self.$apis.bindWechat(c_postData, c_callback);
								}
							})
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.login(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
			
			register() {
				const self = this;
				
				const postData = {
					data:self.$Utils.cloneForm(self.registerData)					
				}
				/* postData.smsAuth = {						
					phone:self.registerData.phone,						
					code:self.submitData.smsCode,
				}; */
				if(uni.getStorageSync('parent_no')){
					postData.data.parent_no  = uni.getStorageSync('parent_no')
				};
				var newObject = self.$Utils.cloneForm(self.registerData);
				delete newObject.code;
				if (self.$Utils.checkComplete(newObject)) {						
					const callback = (res) => {
						if (res.solely_code == 100000) {
							self.$Utils.showToast(res.msg, 'none');
							self.registerData = {																
								phone:'',
							};
							self.curr = 1;		
						} else {
							self.$Utils.showToast(res.msg, 'none');
						}
					}
					self.$apis.register(postData, callback);
				} else {
					self.$Utils.showToast('请补全信息', 'none');
				};
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

.switchNav .item:first-child{margin-right:80rpx;}
.switchNav .item.on{font-size: 32rpx;font-weight: bold;color: #222;}

.loginEdit{padding: 0 8%;}
.loginEdit .item{padding-top: 40rpx;}
.loginEdit .item .ll{width: 10%;}
.loginEdit .item .icon{width: 44rpx; height: 44rpx;}
.loginEdit .item .rr{width: 90%; padding: 10rpx 0;border-bottom: 1px solid #eee;font-size: 26rpx;}
.loginEdit .item .rr input{width: 100%;line-height: 60rpx;height: 60rpx; display: block;font-size: 24rpx;}
.loginEdit .item .yzm{color: #f6af15;}
.submitbtn{margin-top: 160rpx;}
</style>

