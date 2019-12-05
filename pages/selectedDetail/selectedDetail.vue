<template>
	<view>
		
		<view class="swiperContainer">
		  <swiper @change="swiperChange" autoplay="true" interval="3000" duration="1000" >
		      <swiper-item v-for="(item,index) in imgUrls" :key="index">
		        <image :src="item" class="slide-image img"  />
		      </swiper-item>
		  </swiper>
		  <view class="imageCount">{{current+1}}/{{imgUrls.length}}</view>
		</view>
		
		<view class="pdlr4 fixBox">
			<view class="textBox fs13 white mgb25">汾阳王在之前应该和杏花村都属于山西名酒，清香典范，老牌子新产品。时间黑瓶包装精美，尤其是瓶子，可以说这款酒的味道是一方面，酒体由于度数原因略薄，入口柔顺，清爽，清香浓郁。</view>
			<view class="seltDetail flexRowBetween whiteBj radius10" style="align-items: flex-start;">
				<view class="pic">
					<image src="../../static/images/home-img10.png" mode=""></image>
				</view>
				<view class="infor pr">
					<view class="title avoidOverflow fs13">50°汾阳王青花10 500ml</view>
					<view class="flexRowBetween B-price">
						<view class="price fs15">56.00</view>
						<view class="goBtn fs12" @click="Router.redirectTo({route:{path:'/pages/prodetail/prodetail'}})">去看看</view>
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
				imgUrls: [
					'../../static/images/home-select-img3.png',
					'../../static/images/home-select-img3.png',
					'../../static/images/home-select-img3.png'
				],
				current: 0
			}
		},
		 swiperChange(e) {
		    const self = this;
		    if (e.detail.source == 'touch') {
		      self.setData({
		        current: e.detail.current
		      })
		    }
		 },
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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

.fixBox{position: fixed;right: 0;bottom: 30rpx;left: 0;z-index: 10;}
.seltDetail .pic{width: 180rpx; height: 180rpx;}
.seltDetail .pic image{width: 100%;height: 100%;display: block;}
.seltDetail .infor{width: 70%; height: 180rpx;padding: 20rpx 30rpx 20rpx 0;box-sizing: border-box;}
.seltDetail .B-price{position: absolute; bottom: 30rpx;left: 0;right: 30rpx;}
.seltDetail .goBtn{width: 100rpx;height: 44rpx;border-radius: 8rpx;background: #FF2121;color: #fff;color: #fff;text-align: center;}

/* 大图片轮播 */
.swiperContainer {position: fixed;top: 0;right: 0;bottom: 0;left: 0;}
swiper,swiper-item{width: 100%;height:100%;}
.swiperContainer image{ width: 100%;height:100%; display: block;}
.imageCount {width:80rpx; height:80rpx; background-color: rgba(0, 0, 0, 0.5); border-radius:40rpx; line-height:80rpx; color:#fff; text-align:center; font-size:26rpx; position:absolute; right:40rpx; bottom:40%;}
</style>

