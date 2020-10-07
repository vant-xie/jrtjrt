<template>
	<view class="shangpinzhongxin">
		<view class="shangpinzhongxin-top">
			<scroll-view scroll-x="true" class="scroll">
				<view :class="['tabar1',index==actIndex?'active':'']" v-for="(item,index) in tabsList" :key='index' :data-id='item.id'
				 :data-index='index' @click="btn_nav">{{item.name}}</view>
			</scroll-view>
		</view>
		<view class="shangpinzhongxin-main">
			<view class="shangpin-content">
				<view class="shangpin-item" v-for="(item,index) in shangpinList" :key='item.id' :data-id='item.id' @click="details">
					<view class="shangpin-item-top">
						<image :src="item.pic"></image>
					</view>
					<view class="shangpin-item-footer">
						<view class="shangpin-item-footer-text1">{{item.name}}</view>
						<view class="shangpin-item-footer-text2">
							<view class="shangpin-item-footer-textl">￥</view>
							<view class="shangpin-item-footer-textr">{{item.money}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		onLoad() {
			// var id=options.id;
			// console.log(id)
			var _that = this;
			uni.request({
					url: 'https://www.jrtjrt.top/index/goods_sort/goods_sort_lst',
					data: {

					},
					methods: "post",
					success: (res) => {

						// console.log(res.data.data);

						var data = res.data.data;
						_that.tabsList = data;
						// console.log(_that.tabsList)
						// console.log(_that.goods_detail)
						// _that.lun_Img = data;
					}
				}),
				uni.request({
					url: 'https://www.jrtjrt.top/index/goods/goods_lst',
					data: {

					},
					methods: "post",
					success: (res) => {

						// console.log(res);

						var data = res.data.data.data;
						_that.shangpinList = data;
						// console.log(_that.shangpinList)
						// console.log(_that.goods_detail)
						// _that.lun_Img = data;
					}
				})
		},
		//分享
		onShareAppMessage() {
			
		},
		
		data() {
			return {
				actIndex: 0,
				tabsList: '',
				shangpinList: '',
				id: '',
				current:1,
				length:12
				// shangpinList:[{
				// 	id:11,
				// 	src:"../../static/img/shangpin/shangpin-1.png",
				// 	goodsName:"什么商品",
				// 	price:"258",
				// },{
				// 	id:11,
				// 	src:"../../static/img/shangpin/shangpin-1.png",
				// 	goodsName:"什么商品",
				// 	price:"258",
				// },{
				// 	id:11,
				// 	src:"../../static/img/shangpin/shangpin-1.png",
				// 	goodsName:"什么商品",
				// 	price:"258",
				// },{
				// 	id:11,
				// 	src:"../../static/img/shangpin/shangpin-1.png",
				// 	goodsName:"什么商品",
				// 	price:"258",
				// },{
				// 	id:11,
				// 	src:"../../static/img/shangpin/shangpin-1.png",
				// 	goodsName:"什么商品",
				// 	price:"258",
				// }]
			}
		},
		// onReachBottom(){
		// 	// console.log(2)
		// 	var _that = this;
		// 	// console.log(this.current)
		// 	uni.request({
		// 		url: 'https://www.jrtjrt.top/index/goods/goods_lst',
		// 		data: {
		// 			user_id:this.user_id,
		// 			current:this.current,
		// 			length:this.length
		// 		},
		// 		method:'POST',
		// 		success: (res) => {
		// 			// console.log(res);
		// 			var data = res.data.data.data;
		// 			// _that.shangpinList = data;
		// 			if(data.current==1){
		// 				_that.shangpinList = data;
		// 				_that.current = _that.current + 1;
		// 			}else{
		// 				if(data.length==0){
		// 					uni.showToast({
		// 						icon:'none',
		// 						title:'没有更多内容了'
		// 					})
		// 				}else{
		// 					_that.shangpinList = _that.shangpinList.concat(data);
		// 					_that.current = _that.current + 1;
		// 				}
						
		// 			}
					
		// 			// console.log(_that.shangpinList)
		// 			// this.text = 'request success';
		// 		}
		// 	});
		// },
		methods: {
			//商品列表
			btn_nav(e) {

				this.actIndex = e.currentTarget.dataset.index || e.target.dataset.index;
				this.id = e.currentTarget.dataset.id || e.target.dataset.id;
				// this.id = e.currentTarget.dataset.id||e.target.dataset.id;
				// console.log(this.actIndex);
				// console.log(this.id);
				var _that = this;
				if (this.actIndex == 0) {
					uni.request({
						url: 'https://www.jrtjrt.top/index/goods/goods_lst',
						data: {
							// goods_sort_id:_that.id
						},
						methods: "post",
						success: (res) => {
							// console.log(res);						
							var data = res.data.data.data;
							_that.shangpinList = data;
						}
					})
				} else {
					uni.request({
						url: 'https://www.jrtjrt.top/index/goods/goods_lst',
						data: {
							goods_sort_id: _that.id
						},
						methods: "post",
						success: (res) => {
							// console.log(res);						
							var data = res.data.data.data;
							_that.shangpinList = data;
						}
					})
				}

			},
			//进入商品详细
			details(e) {
				let id = e.currentTarget.dataset.id || e.target.dataset.id;
				uni.navigateTo({
					url: '/pages/details/details?id=' + id
				})
			}
		}

	}
</script>

<style>
	.shangpinzhongxin {
		background-color: #F7F7F7;
		width: 100%;
	}

	.shangpinzhongxin-top {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 80upx;
		background-color: #FFFFFF;
	}

	.scroll {
		display: flex;
		flex-direction: row;
		white-space: nowrap;
	}

	.tabar1 {
		display: inline-block;
		width: 20%;
		/* padding-left: 20upx; */
		font-size: 28upx;
		color: #616161;
		line-height: 80upx;
		position: relative;
		text-align: center;
	}

	.tabar1:first-child {
		color: #000000;
	}

	.tabar1.active::after {
		content: "";
		position: absolute;
		width: 28upx;
		height: 0;
		left: 50%;
		margin-left: -14upx;
		bottom: 0;
		border-bottom: 4upx solid #F21C24;
	}

	.shangpin-content {
		width: 100%;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		margin-top: 80upx;
	}

	.shangpin-item {
		width: 370upx;
		height: 504upx;
		background-color: #FFFFFF;
		margin: 0 10upx 10upx 0;

	}

	.shangpin-item:nth-child(even) {
		margin-right: 0;
	}

	.shangpin-item-top {
		width: 360upx;
		height: 360upx;
	}

	.shangpin-item-top image {
		width: 100%;
		height: 100%;
	}

	.shangpin-item-footer {
		width: 80%;
		margin-left: 30upx;
	}

	.shangpin-item-footer-text1 {
		/* width: 222upx; */
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		/* height: 30upx; */
		font-size: 32upx;
		color: #202020;
		margin: 10upx 0 0 0;
	}

	.shangpin-item-footer-text2 {
		width: 100%;
		color: #FF3434;
		display: flex;
		flex-direction: row;
		padding: 20upx 0 20upx 0;
	}

	.shangpin-item-footer-textl {
		width: 16upx;
		height: 26upx;
		font-size: 26upx;
		line-height: 22upx;
		padding-top: 12upx;
		padding-right: 10upx;
	}

	.shangpin-item-footer-textr {
		width: 72upx;
		height: 26upx;
		font-size: 32upx;
		margin-left: 10upx;
	}
</style>
