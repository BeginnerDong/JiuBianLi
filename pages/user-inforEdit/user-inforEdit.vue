<template>
	<view>
		
		<view class="pdlr4">
			<view class=" myRowBetween">
				<view class="item flexRowBetween">
					<view class="ll">会员名称</view>
					<view class="rr">
						<input type="text" value="" v-model="submitData.name" placeholder="请输入会员名称" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">性别</view>
					<view class="rr"  @click="setShow">
						<view class="color9">{{submitData.gender==1?'男':(submitData.gender==2?'女':'请选择您的性别')}}</view>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">生日</view>
					<view class="rr">
						<picker mode="date"   @change="bindDateChange">
							<view class="color9">{{birth!=''?birth:'请选择您的生日'}}</view>
						</picker>
						<!-- <view class="color9">请选择您的生日</view> -->
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="pdtb15 flexEnd fs12 color6">
				<view>填写生日有惊喜，保存一年之内不可更改！</view>
			</view>
		</view>
		
		 
		<view class="submitbtn" style="margin-top: 300rpx;">
			<button class="btn" type="button" @click="submit">保存</button>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="alertBox pdlr4 whiteBj radius10" v-show="is_setShow">
			<view class="pdtb15 borderB1" @click="changeGender('1')">男</view>
			<view class="pdtb15" @click="changeGender('2')">女</view>
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
				is_setShow:false,
				submitData:{
					gender:'',
					birth:'',
					name:'',
					age:''
				},
				birth:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			 bindDateChange: function(e) {
				const self = this;
				var date=new Date;
				var y = date.getFullYear()
				var array = e.target.value.split('-');
				self.submitData.age = parseInt(y)-parseInt(array[0]);
				self.birth = e.target.value;
				self.submitData.birth = self.$Utils.timeToTimestamp(e.target.value)
			},
			
			setShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_setShow = !self.is_setShow
			},
			
			changeGender(index){
				const self = this;
				self.submitData.gender = index;
				self.is_show = !self.is_show;
				self.is_setShow = !self.is_setShow
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.name = self.mainData.name;
						self.submitData.birth = self.mainData.birth;
						self.submitData.gender = self.mainData.gender;
						self.birth = self.$Utils.timeto(self.submitData.birth*1000,'ymd')
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					self.userInfoUpdate();	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('保存成功', 'none');
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
				self.$apis.userInfoUpdate(postData, callback);
			},
		}
	};
</script>

<style>
@import "../../assets/style/editInfor.css";
.myRowBetween .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}

.alertBox{width: 80%; position: fixed;top: 50%;left:50%;transform: translate(-50%,-50%);box-sizing: border-box; z-index: 50;}
</style>

