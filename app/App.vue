
<script>
	import token from "./common/token.js"
	export default {
		onLaunch: function() {	
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	@import "/assets/style/style.css";
	@import "/assets/style/quill.css";
	
	uni-tabbar .uni-tabbar__icon{width: 44rpx;height: 44rpx;margin-top: 8rpx;}
	uni-page-head .uni-page-head__title{font-weight: normal;}
	uni-tabbar .uni-tabbar__label{line-height: 30rpx; margin-top: 10rpx;}
	button:after{border: none;}
	.ql-editor img{max-width: 100%;margin: 0 auto;display: block;}
	uni-swiper .uni-swiper-dots-horizontal{ line-height: 16rpx;height: 16rpx;}
	.index_topBj uni-swiper .uni-swiper-dot{background: rgba(255,255,255,0.5);}
	uni-input{font-size: 26rpx;}
	uni-input-placeholder{color: #999;}
</style>
