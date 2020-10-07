<template>
	<view class="details">
		<view class="details-screen">
			<view class="details-top">
				<view class="goods_detail">
					<swiper indicator-dots>
						<swiper-item v-if="goods_detail.video">
							<video :src="goods_detail.video" show-mute-btn="true" muted="true"></video>
						</swiper-item>
						<swiper-item v-for="(item,index) in goods_detail.img" :key="index" indicator-dots="false" autoplay="true"
						 interval="3000" duration="500">
							<image :src="item"></image>
						</swiper-item>
					</swiper>
				</view>
				<view class="details-top-text">
					<view class="details-top-t">
						<view class="details-top-t1">{{goods_detail.name}}</view>
						<view class="details-top-t2"><text class="details-top-t2-text1">￥{{goods_detail.money}}</text>
						</view>
						<text class="details-top-t2-text2">会员价￥{{goods_detail.member_money}}</text>
						<view class="details-top-l">
							<view class="details-top-l-icon">
								<image src="../../static/img/shangpinxx/icon-fx.png"></image>
							</view>
							<text>分享</text>
							<button class="details-btn" type="default" open-type="share"></button>
						</view>

					</view>
					<view class="details-top-f">
						<text>运费：￥{{goods_detail.fare}}</text>
						<text>销量：{{goods_detail.sales}}</text>
						<text>库存：{{goods_detail.stock}}</text>
					</view>
				</view>
			</view>
			<view class="details-cont">
				<text class="details-cont-text1">选择</text>
				<text class="details-cont-text2" @click="btn_guige()">规格</text>
				<view class="details-cont-icon" @click="btn_guige()">
					<image src="../../static/img/shangpinxx/icon-jt.png"></image>
				</view>
			</view>
			<view class="details-main">
				<text class="details-main-text">商品详情</text>
				<view class="details-main-item" v-html="goods_detail.content"></view>
			</view>
		</view>

		<!--弹出框  -->
		<view class="commodity-attr-box" v-if="showModalStatus" catchtouchmove="true" :animation="animationData">
			<view class="commodity-top">
				<view class="commodity-Img">
					<image :src="goods_detail.img[0]"></image>
				</view>
				<view class="column">
					<text class="sku-price">￥{{goods_detail.money}}</text>
					<text class="sku-title">库存：{{goods_detail.stock}}</text>
				</view>
				<view class="column-icon" @click="btn_del()">
					<image src="../../static/img/shangpinxx/icon-x.png"></image>
				</view>
			</view>
			<text class="line"></text>
			<view class="guige" v-for="(item2,index) in goods_detail.specs_sort" :key="index">
				<text class="guige-title">{{item2.name}}</text>
				<view class='gg'>
					<text v-for="(item3,index) in item2.specs" :key="index" :class="index==gg_id?'active':''" :data-id="item3.id"
					 :data-price="item3.money" @click="filter" :data-index="index">
						{{item3.name}}
					</text>
				</view>
			</view>
			<text class="line"></text>
			<view class="row">
				<text class='quantity-title'>数量</text>
				<view class="quantity-position">
					<view class="stepper">
						<!-- 减号 -->
						<text :class="minusStatus" @click="bindMinus()">-</text>
						<!-- 数值 -->
						<input type="number" disabled=true :value="num" />
						<!-- 加号 -->
						<text class="normal" @click="bindPlus()">+</text>
					</view>
				</view>
			</view>
			<text class="line"></text>
		</view>
		<!-- footer -->
		<view class="details-footer">
			<view class="details-footer-icon" @click="btn_shoye">
				<image src="../../static/img/shangpinxx/icon-shoy.png"></image>
				<text>首页</text>
			</view>
			<view class="details-footer-icon">
				<image src="../../static/img/shangpinxx/icon-kf.png"></image>
				<text>客服</text>
				<button class="details-btnkf" open-type="contact"></button>
			</view>
			<text class="details-footer-text1" @click="btn_addshopping()">加入购物车</text>
			<text class="details-footer-text2" v-if="!showModalStatus" @click="Buy1">立即购买</text>
			<text class="details-footer-text2" v-if="showModalStatus" @click="Buy(1)">立即购买</text>
		</view>
		<view v-if="showModalStatus" style="position: fixed;top: 0;left: 0;right: 0;bottom: 0;width: 100%;height: 80%;background: rgba(0,0,0,.5);z-index: 999;"
		 catchtouchmove="true"></view>
	</view>
</template>

<script>
	export default {
		onLoad(options) {

			// console.log(user_id);
			var id = options.id;
			this.id = options.id;
			this.uid = options.uid;
			if (options.uid) {
				uni.setStorageSync("fx_id", options.uid)
			}
			// console.log(id)
			var _that = this;
			uni.request({
				url: 'https://www.jrtjrt.top/index/goods/goods_lst_d',
				data: {
					id: id
				},
				methods: "post",
				success: (res) => {
					
					var data = res.data.data;
					data.content = data.content.replace(/<img/g,'<img style="width: 100%;"')
					_that.goods_detail = data;
					// console.log(_that.goods_detail)
					_that.specs_id = data.specs_sort[0].specs[0].id;
					_that.member_money=data.member_money;
					// console.log(_that.member_money)

				}
			})
		},
		onShow() {
			this.user_id = uni.getStorageSync('user_id');
		},
		data() {
			return {
				goods_detail: '',
				showModalStatus: false, //是否显示
				animationData: {}, //动画
				gg_id: 0, //规格ID
				gg_txt: '', //规格文本
				gg_price: 0, //规格价格
				num: 1, //初始数量
				minusStatus: 'disabled',
				goods_id: '', //商品id
				specs_id: 1, //规格id
				member_money:''

			}
		},
		onShareAppMessage() {
			// console.log(this.id)
			return {
				title: this.goods_detail.name,
				path: "/pages/details/details?id=" + this.id + "&uid=" + this.user_id
			}
		},
		methods: {
			//分享
			// shareFriend() {
			// 	//分享到微信聊天，图文分享
			// 	uni.share({
			// 		provider: "weixin",
			// 		scene: "WXSceneSession",
			// 		type: 5,
			// 		title: "uni-app分享",
			// 		imageUrl: "https:///ht/goodsimg/20200817/4abd4a714dbc9392046a7e0a39adca20.png",
			// 		miniProgram: {
			// 			id: 'wx331944e9ed0ee9db',
			// 			path: 'pages/details/details',
			// 			type: 1
			// 		},					
			// 		success: function(res) {
			// 			console.log("success:" + JSON.stringify(res));
			// 		},
			// 		fail: function(err) {
			// 			console.log("fail:" + JSON.stringify(err));
			// 		}
			// 	});
			// 	//分享到微信朋友圈
			// 	// uni.share({
			// 	// 	provider: "weixin",
			// 	// 	scene: "WXSceneSession",
			// 	// 	type: 0,
			// 	// 	href: "http://uniapp.dcloud.io/", //这里写打开app的urlschemes
			// 	// 	title: "uni-app分享",
			// 	// 	summary: "我正在使用HBuilderX开发uni-app，赶紧跟我一起来体验！",
			// 	// 	imageUrl: "https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png",
			// 	// 	success: function(res) {
			// 	// 		console.log("success:" + JSON.stringify(res));
			// 	// 	},
			// 	// 	fail: function(err) {
			// 	// 		console.log("fail:" + JSON.stringify(err));
			// 	// 	}
			// 	// });
			// },
			btn_del() {
				this.showModalStatus = false;
			},
			btn_guige() {
				this.showModalStatus = true;
			},

			//加了购物车
			btn_addshopping() {
				if (this.user_id) {
					if (this.showModalStatus == false) {
						this.showModalStatus = true
					} else if (this.showModalStatus == true) {
						var num = this.num;
						var goods_id = this.goods_detail.id;
						var specs_id = this.specs_id;
						uni.request({
							url: 'https://www.jrtjrt.top/index/shopping_cart/shopping_cart_add',
							data: {
								user_id: this.user_id,
								goods_id: goods_id,
								specs_id: specs_id,
								num: num
								// code:code
							},
							methods: "post",
							success: (res) => {
								// console.log(res.data.code);
								this.code = res.data.code;
								// console.log(this.code)
								if (this.code == 1) {
									this.showModalStatus = false;
									uni.showToast({
										icon: "none",
										title: res.data.message
									})
								} else if (this.code == 0) {
									uni.showToast({
										icon: "none",
										title: res.data.message
									})
								}

								// var data = res.data.data.data;
								// _that.shangpinList = data;
							}
						})
					}
				} else {
					uni.switchTab({
						url: '../user/user'
					});
					uni.showToast({
						icon: "none",
						title: "请微信授权"
					})
				}
			},
			Buy(ask) {
				if (this.user_id) {
					
					// console.log(this.member_money)
					
						uni.navigateTo({
							url: '../jiesuan/jiesuan?ask=' + ask + "&num=" + this.num + "&specs_id=" + this.specs_id + "&id=" + this.goods_detail
								.id + "&member_money="+this.member_money
						})
					
					
				} else {
					uni.switchTab({
						url: '../user/user'
					});
					uni.showToast({
						icon: "none",
						title: "请微信授权"
					})
				}

				// console.log(1)
			},
			Buy1(){
				this.showModalStatus=true;
			},
			// 规格选中
			filter(e) {
				var self = this;

				// console.log(this.goods_detail.specs_sort[0].specs);
				this.gg_id = e.currentTarget.dataset.index || e.target.dataset.index;
				this.id = e.currentTarget.dataset.id || e.target.dataset.id;
				this.specs_id = this.id;
				// console.log(this.specs_id)

			},
			/* 点击减号 */
			bindMinus() {
				// console.log(this.num)
				var num = this.num;
				if (num > 1) {
					num--;
				}
				var minusStatus = num <= 1 ? 'disabled' : 'normal';
				this.num = num;
				this.minusStatus = minusStatus;
			},
			/* 点击加号 */
			bindPlus() {
				var num = this.num;
				// 不作过多考虑自增1  
				num++;
				// 只有大于一件的时候，才能normal状态，否则disable状态  
				var minusStatus = num < 1 ? 'disabled' : 'normal';
				// 将数值与状态写回  
				this.num = num
				this.minusStatus = minusStatus
			},
			//回首页
			btn_shoye() {
				uni.switchTab({
					url: '../main/main'
				})
			}
		}
	}
</script>

<style>
	.details-main-item img{
		width: 100%;
	}
	.details {
		width: 100%;
		background-color: #F3F3F3;
	}

	/*使屏幕变暗  */
	.commodity_screen {
		width: 100%;
		height: 100%;
		position: fixed;
		top: 0;
		left: 0;
		background-color: #000000;
		opacity: 0.5;
		overflow: hidden;
		z-index: 999;
		color: #fff;
	}

	.goods_detail {
		width: 750upx;
		height: 750upx;
	}

	.goods_detail>swiper {
		width: 100%;
		height: 100%;
	}

	.goods_detail image {
		width: 100%;
		height: 100%;
	}
	.goods_detail video{
		width: 100%;
		height: 100%;
	}

	.details-btn {
		width: 136upx;
		height: 60upx;
		position: absolute;
		/* top: 10upx; */
		top: 0;
		right: 0;
		opacity: 0.1;
	}

	/* details-top */
	.details-top {
		background-color: #FFFFFF;
	}

	.details-top-text {
		width: 750upx;
		height: 226upx;
		border-radius: 30upx 0 0 30upx;
		position: relative;
	}

	.details-top-t {
		height: 170upx;
	}

	.details-top-t1 {
		width: 552upx;
		/* height: 30upx; */
		word-wrap: break-word;
		font-size: 28upx;
		color: #333333;
		margin: 26upx 0 10upx 30upx;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
		overflow: hidden;
	}

	.details-top-t2 {
		margin: 26upx 0 10upx 30upx;
		width: 400upx;
	}

	.details-top-t2-text1 {
		font-size: 34upx;
		color: #FF3434;
	}

	.details-top-t2-text2 {
		font-size: 26upx;
		color: #666666;
		padding-left: 30upx;
		/* display: inline-block; */
	}

	.details-top-l {
		display: flex;
		flex-direction: row;
		width: 136upx;
		height: 60upx;
		/* float: right; */
		position: absolute;
		top: 10upx;
		right: 0;
		/* margin-top: -140upx; */
		border-radius: 30upx 0 0 30upx;
		background-color: #E6E6E6;
		color: #666666;
		font-size: 24upx;
		line-height: 60upx;
		text-align: center;
	}

	.details-top-l-icon {
		width: 26upx;
		height: 26upx;
		margin: 3upx 8upx 14upx 35upx;
	}

	.details-top-l-icon>image {
		width: 100%;
		height: 100%;
		/* margin-top: 6upx; */
	}

	.details-top-f {
		display: flex;
		justify-content: space-between;
		padding: 0 30upx 0 30upx;
	}

	.details-top-f>text {
		font-size: 24upx;
		color: #999999;
	}

	/* details-cont */
	.details-cont {
		margin-top: 20upx;
		background-color: #FFFFFF;
		height: 88upx;
		width: 100%;
		line-height: 88upx;
	}

	.details-cont-text1 {
		font-size: 28upx;
		color: #999999;
		padding-left: 30upx;
	}

	.details-cont-text2 {
		font-size: 24upx;
		color: #333333;
		padding-left: 30upx;
		display: inline-block;
		width: 560upx;
	}

	.details-cont-icon {
		float: right;
		margin-right: 30upx;
	}

	.details-cont-icon>image {
		width: 16upx;
		height: 28upx;
	}

	/* main */
	.details-main {
		margin-top: 20upx;
		margin-bottom: 98upx;
		background-color: #FFFFFF;
	}

	.details-main-item {
		/* margin-left: 30upx; */
	}

	.details-main-text {
		font-size: 28upx;
		color: #333333;
		margin-left: 32upx;
		margin-top: 20upx;
		margin-bottom: 20upx;
		display: inline-block;
	}

	.details-main-img {
		width: 750upx;
		height: 750upx;
	}

	.details-main-img>image {
		width: 750upx;
		height: 750upx;
	}

	/* 弹出框 */
	.commodity-attr-box {
		width: 94%;
		position: fixed;
		bottom: 98upx;
		left: 0;
		z-index: 2000;
		background: #FFFFFF;
		padding: 30upx 30upx 0 30upx;
	}

	.commodity-top {
		display: flex;
		flex-direction: row;
	}

	.commodity-Img {
		width: 140upx;
		height: 140upx;
		border: 2upx solid #f5f5f5;
		z-index: 12;
		/* 		margin-top: 30upx;
		margin-left: 30upx; */

	}

	.commodity-Img image {
		width: 100%;
		height: 100%;
	}

	.column {
		display: flex;
		flex-direction: column;

	}

	.column-icon {
		width: 28upx;
		height: 28upx;
		position: absolute;
		top: 86upx;
		right: 50upx;
	}

	.column-icon>image {
		width: 100%;
		height: 100%;
	}

	.sku-price {
		color: #FF3434;
		margin: 32upx 14upx 14upx 30upx;
		font-size: 34upx;
	}

	.sku-title {
		font-size: 24upx;
		margin: 0 14upx 14upx 30upx;
		color: #999999;
	}

	.line {
		width: 95%;
		height: 2upx;
		display: inline-block;
		margin: 30upx 0upx 5upx 0upx;
		background-color: #f2f2f2;
		text-align: center;
	}

	.guige-title {
		font-size: 24upx;
		color: #333333;
	}

	.gg text {
		/* width: 100upx;
		height: 50upx; */
		padding: 0 20upx;
		line-height: 50upx;
		float: left;
		background: #f2f2f2;
		text-align: center;
		margin: 20upx 0 0 26upx;
		font-size: 24upx;
		border-radius: 10upx;
	}

	.gg .active {
		color: #FFFFFF;
		background-color: #FF3434;
	}

	.row {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-between;
		padding: 0 30upx 0 30upx;
	}

	.quantity-title {
		font-size: 24upx;
		color: #333333;
		text-align: center;
		margin-top: 10upx;
	}

	.quantity-position {}

	.stepper {
		width: 196upx;
		height: 50upx;
		border: 2upx solid #E5E5E5;
		display: flex;
		flex-direction: row;
	}

	.stepper .normal {
		color: black;
	}

	.stepper .disabled {
		color: #999999;
	}

	/* 加号和减号 */
	.stepper text {
		width: 33.3%;
		height: 50upx;
		line-height: 50upx;
		text-align: center;
		font-size: 36upx;
		display: inline-block;
	}

	.stepper input {
		width: 33.3%;
		height: 50upx;
		text-align: center;
		border-left: 2upx solid #E5E5E5;
		border-right: 2upx solid #E5E5E5;
	}

	/* footer */
	.details-footer {
		position: fixed;
		display: flex;
		flex-direction: row;
		width: 100%;
		height: 98upx;
		bottom: 0;
		width: 100%;
		z-index: 999;
		background-color: #FFFFFF;
	}

	.details-footer-icon {
		width: 20%;
		height: 98upx;
		margin-top: 24upx;
		display: flex;
		flex-direction: column;
		position: relative;
	}

	.details-btnkf {
		width: 36upx;
		height: 60upx;
		position: absolute;
		top: 0;
		left: 0;
		opacity: 0.1;
	}

	.details-footer-icon:first-child {
		padding-left: 74upx;
	}

	.details-footer-icon:last-child {
		padding-left: 44upx;
	}

	.details-footer-icon>image {
		width: 36upx;
		height: 32upx;
	}

	.details-footer-icon>text {
		font-size: 22upx;
		color: #666666;
	}

	.details-footer-text1 {
		width: 210upx;
		height: 98upx;
		text-align: center;
		font-size: 28upx;
		color: #FFFFFF;
		line-height: 98upx;
		background-color: #FF9B19;
	}

	.details-footer-text2 {
		width: 210upx;
		height: 98upx;
		text-align: center;
		font-size: 28upx;
		line-height: 98upx;
		color: #FFFFFF;
		background-color: #E01212;
	}
</style>
