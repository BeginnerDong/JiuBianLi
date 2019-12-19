<template>
	<view>
		
		<view class="editLine pdlr4">
			<view class="item flexRowBetween">
				<view class="ll">发票类型</view>
				<view class="rr flex fs13">
					<view class="dzfp-Lab fs13">电子发票</view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">发票信息</view>
				<view class="rr flex fs13">
					<image class="seltIcon mgr5" src="../../static/images/invoice-icon.png" mode=""></image>
					<text>明细</text>
				</view>
			</view>
			<view class="item flexRowBetween" style="border-bottom: 0;">
				<view class="ll">发票抬头</view>
				<view class="rr flex fs13">
					<view class="flex mgr25" @click="changeSelt('1')">
						<image class="seltIcon mgr5" :src="numSelt==1?'../../static/images/invoice-icon.png':'../../static/images/invoice-icon1.png'" mode=""></image>
						<text>个人</text>
					</view>
					<view class="flex mgl15" @click="changeSelt('2')">
						<image class="seltIcon mgr5" :src="numSelt==2?'../../static/images/invoice-icon.png':'../../static/images/invoice-icon1.png'" mode=""></image>
						<text>单位</text>
					</view>
				</view>
			</view>
			<view class="fapInfor f5bj radius10 mgb15">
				<view class="item">
					<input type="text" v-model="submitData.title" placeholder="*(必填)请填写发票抬头" placeholder-class="placeholder" />
				</view>
				<view class="item">
					<input type="text" v-model="submitData.code" placeholder="*(必填)请输入纳税人识别号" placeholder-class="placeholder" />
				</view>
				<view class="item">
					<input type="text" v-model="submitData.company_address" placeholder="请输入单位地址" placeholder-class="placeholder" />
				</view>
				<view class="item">
					<input type="number" v-model="submitData.company_phone" placeholder="请输入单位电话" placeholder-class="placeholder" />
				</view>
				<view class="item">
					<input type="text" v-model="submitData.company_bank" placeholder="请输入单位开户银行名称" placeholder-class="placeholder" />
				</view>
				<view class="item">
					<input type="number" v-model="submitData.company_bank_no" placeholder="请输入单位银行卡账户号码" placeholder-class="placeholder" />
				</view>
			</view>
			
			<view class="borderB1"></view>
			<view class="item flexRowBetween">
				<view class="ll">收票人信息</view>
				<view class="rr flex fs13">
					<input type="text" v-model="submitData.email" placeholder="*(必填)请输入您的收件邮箱" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		
		<view class="twoBtn white flexRowBetween">
			<view class="item left submitbtn">
				<button class="btn" type="button"  @click="Router.back(1)">不开发票</button>
			</view>
			<view class="item right submitbtn">
				<button class="btn" type="button" @click="Utils.stopMultiClick(submit)">确定</button>
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
				numSelt:1,
				submitData:{
					receipt:1,
					title:'',
					code:'',
					company_address:'',
					company_phone:'',
					company_bank:'',
					company_bank_no:'',
					email:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
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
				postData.saveAfter = [
					{
						tableName: 'Order',
						FuncName: 'update',
						data: {
							receipt:1,
							receipt_status:1,
							snap_receipt:self.$Utils.cloneForm(self.submitData)
						},
						searchItem:{
							id:self.id
						}
					}
				];	
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('申请成功', 'none');
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
			
			changeSelt(numSelt){
				const self = this;
				if(numSelt!=self.numSelt){
					self.numSelt = numSelt;
					self.submitData.receipt = numSelt;
				}
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
						self.submitData.title = self.mainData.title;
						self.submitData.code = self.mainData.code;
						self.submitData.company_address = self.mainData.company_address;
						self.submitData.company_phone = self.mainData.company_phone;
						self.submitData.company_bank = self.mainData.company_bank;
						self.submitData.company_bank_no = self.mainData.company_bank_no;
						self.submitData.email = self.mainData.email
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.dzfp-Lab{ line-height: 60rpx;padding: 0 20rpx;border: 1px solid #FF2121;border-radius: 8rpx;color: #FF2121;box-sizing: border-box;}
	.editLine .item{padding: 30rpx 0;border-bottom: 1px solid #e4e4e4;}
	.seltIcon{width: 30rpx;height: 30rpx;display: block;}
	.fapInfor .item{padding: 30rpx 4%;}
	.fapInfor .item input{width: 100%; line-height: 40rpx;height: 40rpx;font-size: 26rpx;}
	.fapInfor .item:last-child{border-bottom: 0;}
	
	.twoBtn{padding: 100rpx 10% 60rpx 10%;}
	.twoBtn .item{width: 300rpx;}
	.twoBtn .item.left .btn{background: #222222;}
</style>

