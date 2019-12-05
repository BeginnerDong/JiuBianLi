<template>
	<view>
		
		<view class="pdlr4">
			<view class=" myRowBetween">
				<view class="item flexRowBetween">
					<view class="ll">会员名称</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入会员名称" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">性别</view>
					<view class="rr"  @click="setShow">
						<view class="color9">请选择您的性别</view>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">生日</view>
					<view class="rr">
						<view class="color9">请选择您的生日</view>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="pdtb15 flexEnd fs12 color6">
				<view>填写生日有惊喜，保存一年之内不可更改！</view>
			</view>
		</view>
		
		 
		<view class="submitbtn" style="margin-top: 300rpx;">
			<button class="btn" type="button" @click="Router.redirectTo({route:{path:'/pages/user-infor/user-infor'}})">保存</button>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="alertBox pdlr4 whiteBj radius10" v-show="is_setShow">
			<view class="pdtb15 borderB1" @click="setShow">男</view>
			<view class="pdtb15" @click="setShow">女</view>
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
				is_setShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			setShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_setShow = !self.is_setShow
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
@import "../../assets/style/editInfor.css";
.myRowBetween .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}

.alertBox{width: 80%; position: fixed;top: 50%;left:50%;transform: translate(-50%,-50%);box-sizing: border-box; z-index: 50;}
</style>

