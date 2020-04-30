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
							<input type="text" v-model="smsCode" placeholder="输入验证码" placeholder-class="placeholder" />
						</view>
							
						<view class="yzm" @click="sendCode()" v-if="!hasSend">{{text}}</view>
						<view class="yzm"  v-else>{{text}}</view>
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
							<input type="text" v-model="smsCodeTwo" placeholder="输入验证码" placeholder-class="placeholder" />
						</view>
						<view class="yzm" @click="sendCodeTwo()" v-if="!hasSendTwo">{{textTwo}}</view>
						<view class="yzm"  v-else>{{textTwo}}</view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn">
				<button class="btn" type="button" @click="Utils.stopMultiClick(register)">注册</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				wx_info:{},
				is_show:false,
				curr:1,
				registerData:{
					phone:''
				},
				submitData:{
					login_name:''
				},
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
				
				currentTimeTwo:61,
				textTwo:'获取验证码',
				hasSendTwo:false,
				
				smsCode:'',
				smsCodeTwo:''
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			sendCodeTwo(){
				var self = this;
				console.log(111)
				if(self.hasSendTwo){
					return;
				};
				var phone = self.registerData.phone;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					
					return;
				}
				var postData = {
					data:{
						phone:self.registerData.phone,
						type:1
					}
				};
				
				var callback = function(res){
					if(res.solely_code==100000){
						uni.setStorageSync('canClick',true);
						uni.hideLoading();
						self.hasSendTwo = true;
						var interval = setInterval(function() {
							self.currentTimeTwo--; //每执行一次让倒计时秒数减一
						
							self.textTwo=self.currentTimeTwo + 's';//按钮文字变成倒计时对应秒数
							
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTimeTwo <= 0) {
								clearInterval(interval)
								
								self.hasSendTwo = false;
								self.textTwo='重新发送';
								self.currentTimeTwo= 61;
								
							}
							
						}, 1000);
					}else{
						uni.hideLoading();
						self.$Utils.showToast('发送失败', 'none', 1000)
					};
				};
				self.$apis.codeGet(postData, callback);
			},
			
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.submitData.login_name;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					
					return;
				}
				var postData = {
					data:{
						phone:self.submitData.login_name,
						type:2
					}
				};
				
				var callback = function(res){
					if(res.solely_code==100000){
						uni.setStorageSync('canClick',true);
						uni.hideLoading();
						self.hasSend = true;
						var interval = setInterval(function() {
							self.currentTime--; //每执行一次让倒计时秒数减一
						
							self.text=self.currentTime + 's';//按钮文字变成倒计时对应秒数
							
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTime <= 0) {
								clearInterval(interval)
								
								self.hasSend = false;
								self.text='重新发送';
								self.currentTime= 61;
								
							}
							
						}, 1000);
					}else{
						uni.hideLoading();
						self.$Utils.showToast('发送失败', 'none', 1000)
					};
				};
				self.$apis.codeGet(postData, callback);
			},
			
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
				postData.smsAuth = {
					phone:self.submitData.login_name,						
					code:self.smsCode,
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
				uni.setStorageSync('canClick', false);
				const postData = {
					data:self.$Utils.cloneForm(self.registerData)					
				}
				postData.smsAuth = {
					phone:self.registerData.phone,						
					code:self.smsCodeTwo,
				};
				if(uni.getStorageSync('parent_no')){
					postData.data.parent_no  = uni.getStorageSync('parent_no')
				};
				var newObject = self.$Utils.cloneForm(self.registerData);
				delete newObject.code;
				if (self.$Utils.checkComplete(newObject)) {						
					const callback = (res) => {
						uni.setStorageSync('canClick', true);
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

