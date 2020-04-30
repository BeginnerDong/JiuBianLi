<template>
	<view>
		
		<view class="swiperContainer">
		  <swiper @change="swiperChange" autoplay="true" interval="3000" duration="1000" >
		      <swiper-item v-for="(item,index) in mainData.bannerImg" :key="index">
		        <image :src="item.url" class="slide-image img"  />
		      </swiper-item>
		  </swiper>
		  <view class="imageCount">{{current+1}}/{{mainData.bannerImg.length}}</view>
		</view>
		
		<view class="pdlr4 fixBox">
			<view class="textBox fs13 white mgb25">{{mainData.description}}</view>
			<view class="seltDetail flexRowBetween whiteBj radius10" style="align-items: flex-start;">
				<view class="pic">
					<image :src="mainData.product&&mainData.product[0]
					&&mainData.product[0].mainImg&&mainData.product[0].mainImg[0]?mainData.product[0].mainImg[0].url:''" mode=""></image>
				</view>
				<view class="infor pr">
					<view class="title avoidOverflow fs13">{{mainData.product&&mainData.product[0]?mainData.product[0].title:''}}</view>
					<view class="flexRowBetween B-price">
						<view class="price fs15">{{mainData.product&&mainData.product[0]?mainData.product[0].price:''}}</view>
						<view class="goBtn fs12" :data-id="mainData.product[0].id"
						@click="Router.redirectTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">去看看</view>
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
				
				current: 0,
				mainData:{}
			}
		},
		 
		onLoad(options) {
			const self = this;
			self.id = options.id;
			if(options.index){
				self.current =parseInt(options.index)
			};
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			swiperChange(e) {
			   const self = this;
			   console.log('e',e)
			   self.current = e.detail.current
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id:self.id
				};
				postData.getAfter = {
					product:{
						tableName:'Product',
						middleKey:'url',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
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

