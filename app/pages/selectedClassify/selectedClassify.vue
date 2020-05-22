<template>
	<view>
		
		<view class="pdlr4 whiteBj">
			<view class="orderNav flex fs14 pdb10">
				<scroll-view scroll-x style="white-space: nowrap;display: flex;">
					<view class="tt" :class="currId==item.id?'on':''" @click="changeCurr(item.id)" 
					v-for="(item,index) in typeData" :key="index">{{item.title}}</view>
				</scroll-view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		
		<view class="interct_idexLis">
			<view class="child"  v-for="item in mainData" >
				<view class="flex" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="photo">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="font13 name">{{item.title}}</view>
				</view>
				<view class="font14 pdtb15" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail?id='+$event.currentTarget.dataset.id}})">真材细做，货真价实-信远斋桂花酸梅汤</view>
				<view class="imgbox">
					<view v-for="(c_item,c_index) in item.bannerImg" :class="item.bannerImg.length==1?'lisOne':(item.bannerImg.length==2?'lisTwo':'lisThree')">
						<image :src="c_item.url" :data-index="c_index"  :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/selectedDetail/selectedDetail?id='+$event.currentTarget.dataset.id+'&index='+$event.currentTarget.dataset.index}})" mode="aspectFill"></image>
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
				typeData:[],
				idArray:[],
				mainData:[],
				currId:-1
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getTypeData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			changeCurr(id){
				const self = this;
				if(self.currId!=id){
					self.currId = id;
					self.getMainData(true)
				}
			},
			
			getTypeData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['品类精选']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.typeData.push.apply(self.typeData, res.info.data);
						self.currId = self.typeData[0].id
					}
					self.getMainData()
					console.log('self.typeData', self.typeData)
					self.$Utils.finishFunc('getTypeData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					menu_id:self.currId
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					
				};
				self.$apis.articleGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/NavTab.css";
	@import "../../assets/style/seach.css";
	@import "../../assets/style/selected.css";
	
	page{padding-bottom: 30rpx;background: #F5F5F5;}
	.orderNav .tt{width: auto; margin-right:50rpx;display: inline-block;}
	
</style>

