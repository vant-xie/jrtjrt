<template>
	<view class="order">
		<view class="order-head">
			<view class="order-headbtn">
				<view :class="['order-btn',isActive==0?'active':'']" @click="btn_nav1(0)">待付款</view>
			</view>
			<view class="order-headbtn">
				<view :class="['order-btn',isActive==1?'active':'']" @click="btn_nav1(1)">待发货</view>
			</view>
			<view class="order-headbtn">
				<view :class="['order-btn',isActive==2?'active':'']" @click="btn_nav1(2)">待收货</view>
			</view>
			<view class="order-headbtn">
				<view :class="['order-btn',isActive==3?'active':'']" @click="btn_nav1(3)">已完成</view>
			</view>
		</view>
		<view class="order-centre" v-if="isActive==0">
			<view class="order-item" v-for="(item,index) in order_list" :key="index" v-if="isShow==1">
				<view class="order-main1" v-for="(item1,index) in item.goods" :key="index">
					<view class="order-main1-top">
						<view class="order-main1-l">
							<image :src="item1.pic"></image>
						</view>
						<view class="order-main1-r">
							<view class="order-main1-r-top">
								<text class="order-main1-text1">{{item1.goods_name}}</text>
								<text class="order-main1-text2">{{item1.specs_name}}</text>
							</view>
							<view class="order-main1-r-bottom">
								<text class="order-main1-text3">¥ {{item1.money}}</text>
								<text class="order-main1-text4">x{{item1.num}}</text>
							</view>
						</view>
					</view>
				</view>
				<view class="order-lien">
					<text class="order-lien-text1">配送费</text>
					<text class="order-lien-text2">{{item.priceAll}}元</text>
				</view>
				<view class="order-fahuo">
					<text class="order-fahuo-text1">总价￥{{item.total_money}}</text>
					<text class="order-fahuo-text2">需付款￥{{item.total_money}}</text>
				</view>
				<view class="order-centre-footer">
					<view class="order-centre-footer-text1" :data-order_number=item.order_number @click="order_del">取消订单</view>
					<view class="order-centre-footer-text2" :data-order_number="item.order_number" @click="order_payment">立即付款</view>
				</view>
			</view>
			<view class="order-lack" v-if="isShow==0">
				<view class="order-lack-img">
					<image src="../../static/img/gocart/gocart-lack.png"></image>
				</view>
				<text>订单是空的哦</text>
			</view>
		</view>
		<view class="order-centre" v-if="isActive==1">
			<view class="order-item" v-for="(item,index) in order_list" :key="index" v-if="isShow==1">
				<view class="order-main1" v-for="(item1,index) in item.goods" :key="index">
					<view class="order-main1-top">
						<view class="order-main1-l">
							<image :src="item1.pic"></image>
						</view>
						<view class="order-main1-r">
							<view class="order-main1-r-top">
								<text class="order-main1-text1">{{item1.goods_name}}</text>
								<text class="order-main1-text2">{{item1.specs_name}}</text>
							</view>
							<view class="order-main1-r-bottom">
								<text class="order-main1-text3">¥ {{item1.money}}</text>
								<text class="order-main1-text4">x{{item1.num}}</text>
							</view>
						</view>
					</view>
				</view>
				<view class="order-lien">
					<text class="order-lien-text1">配送费</text>
					<text class="order-lien-text2">{{item.priceAll}}元</text>
				</view>
				<view class="order-fahuo">
					<text class="order-fahuo-text1">总价￥{{item.total_money}}</text>
					<text class="order-fahuo-text2">实付款￥{{item.total_money}}</text>
				</view>
			</view>
			<view class="order-lack" v-if="isShow==0">
				<view class="order-lack-img">
					<image src="../../static/img/gocart/gocart-lack.png"></image>
				</view>
				<text>订单是空的哦</text>
			</view>
		</view>
		<view class="order-centre" v-if="isActive==2">
			<view class="order-item" v-for="(item,index) in order_list" :key="index" v-if="isShow==1">
				<view class="order-main1" v-for="(item1,index) in item.goods" :key="index">
					<view class="order-main1-top">
						<view class="order-main1-l">
							<image :src="item1.pic"></image>
						</view>
						<view class="order-main1-r">
							<view class="order-main1-r-top">
								<text class="order-main1-text1">{{item1.goods_name}}</text>
								<text class="order-main1-text2">{{item1.specs_name}}</text>
							</view>
							<view class="order-main1-r-bottom">
								<text class="order-main1-text3">¥ {{item1.money}}</text>
								<text class="order-main1-text4">x{{item1.num}}</text>
							</view>
						</view>
					</view>
				</view>
				<view class="order-lien">
					<text class="order-lien-text1">配送费</text>
					<text class="order-lien-text2">{{item.priceAll}}元</text>
				</view>
				<view class="order-fahuo">
					<text class="order-fahuo-text1">总价￥{{item.total_money}}</text>
					<text class="order-fahuo-text2">实付款￥{{item.total_money}}</text>
				</view>
				<view class="order-centre-footer">
					<view class="order-centre-footer-text1" @click="btn_logistics(item)">查看物流</view>
					<view class="order-centre-footer-text2" :data-order_number="item.order_number" @click="btn_Receiving">确定收货</view>
				</view>
			</view>
			<view class="order-lack" v-if="isShow==0">
				<view class="order-lack-img">
					<image src="../../static/img/gocart/gocart-lack.png"></image>
				</view>
				<text>订单是空的哦</text>
			</view>
		</view>
		<view class="order-centre" v-if="isActive==3">
			<view class="order-item" v-for="(item,index) in order_list" :key="index" v-if="isShow==1">
				<view class="order-main1" v-for="(item1,index) in item.goods" :key="index">
					<view class="order-main1-top">
						<view class="order-main1-l">
							<image :src="item1.pic"></image>
						</view>
						<view class="order-main1-r">
							<view class="order-main1-r-top">
								<text class="order-main1-text1">{{item1.goods_name}}</text>
								<text class="order-main1-text2">{{item1.specs_name}}</text>
							</view>
							<view class="order-main1-r-bottom">
								<text class="order-main1-text3">¥ {{item1.money}}</text>
								<text class="order-main1-text4">x{{item1.num}}</text>
							</view>
						</view>
					</view>
				</view>
				<view class="order-lien">
					<text class="order-lien-text1">配送费</text>
					<text class="order-lien-text2">{{item.priceAll}}元</text>
				</view>
				<view class="order-fahuo">
					<text class="order-fahuo-text1">总价￥{{item.total_money}}</text>
					<text class="order-fahuo-text2">实付款￥{{item.total_money}}</text>
				</view>
				<view class="order-centre-footer1">
					<view class="order-centre-footer1-text1" @click="btn_logistics(item)">查看物流</view>
					<!-- <view class="order-centre-footer1-text2">确定收货</view> -->
				</view>
			</view>
			<view class="order-lack" v-if="isShow==0">
				<view class="order-lack-img">
					<image src="../../static/img/gocart/gocart-lack.png"></image>
				</view>
				<text>订单是空的哦</text>
			</view>
		</view>
		<!--弹出框  -->
		<view class="order-box" v-if="showModalStatus" catchtouchmove="true">
			<view class="order-box-item">
				<text class="order-box-text1">物流信息</text>
			</view>
			<view class="order-box-icon" @click="btn_close">
				<image src="../../static/img/my/icon-close.png"></image>
			</view>
			<view class="order-box-item1">
				<text class="order-box-text2">物流公司：<text class="order-box-text3">{{logistics_company}}</text> </text>
				<text class="order-box-text4">物流单号：<text class="order-box-text5">{{courier_number}}</text></text>
				<text class="order-box-btn" @click="btn_copy">点击复制快递单号</text>
			</view>
			<button class="order-box-btn1" @click="btn_box">确定</button>
		</view>
		<view v-if="showModalStatus" style="position: fixed;top: 0;left: 0;right: 0;bottom: 0;width: 100%;height: 100%;background: rgba(0,0,0,.5);z-index: 999;"
		 catchtouchmove="true"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showModalStatus: false, //是否显示
				isActive: 0,
				isShow:0,
				order_list: '',
				type: 0,
				logistics_company:'',//快递公司
				courier_number:''//快递单号
			}
		},
		onLoad(options) {
			this.user_id = uni.getStorageSync('user_id');
			this.isActive = options.type;

		},
		onShow() {
			var _that = this;
			this.shuaxin()
		},
		methods: {
			btn_nav1(type) {
				this.isActive = type;

				uni.request({
					url: 'https://www.jrtjrt.top/index/goods_order/goods_order_lst',
					data: {
						user_id: this.user_id,
						type: type
					},
					method: 'POST',
					success: (res) => {
						// console.log(res.data.data.data)
						this.order_list = res.data.data.data;
						// console.log(this.order_list);

						for (var i = 0; i < this.order_list.length; i++) {
							var isfa = 0
							var goods = this.order_list[i].goods
							// console.log(goods)
							for (var j = 0; j < goods.length; j++) {

								isfa += Number(goods[j].fare);
								// console.log(isfa)
							}
							this.order_list[i].priceAll = isfa;
							// console.log(isfa)
						}
						if(res.data.data.data==''){
							this.isShow=0;
							// console.log(1)
							// console.log(this.detriment)
						}else{
							this.isShow=1;
							// console.log(2)
							// console.log(this.detriment)
						}

					}
				})
			},
			//刷新
			shuaxin() {
				var _that = this;
				uni.request({
					url: 'https://www.jrtjrt.top/index/goods_order/goods_order_lst',
					data: {
						user_id: this.user_id,
						type: this.isActive
					},
					method: 'POST',
					success: (res) => {
						// console.log(res.data.data.data)
						this.order_list = res.data.data.data;
						
						for (var i = 0; i < this.order_list.length; i++) {
							var isfa = 0
							var goods = this.order_list[i].goods
							// console.log(goods)
							for (var j = 0; j < goods.length; j++) {

								isfa += Number(goods[j].fare);
								// console.log(isfa)
							}
							this.order_list[i].priceAll = isfa;
							// console.log(isfa)
						}
						if(res.data.data.data==''){
							this.isShow=0;
							// console.log(1)
							// console.log(this.detriment)
						}else{
							this.isShow=1;
							// console.log(2)
							// console.log(this.detriment)
						}

					}
				})
			},
			//订单取消
			order_del(e) {
				this.order_number = e.currentTarget.dataset.order_number || e.target.dataset.order_number;
				// console.log(this.order_number)
				// var order_number=this.order_list
				uni.request({
					url: 'https://www.jrtjrt.top/index/goods_order/goods_order_del',
					data: {
						user_id: this.user_id,
						order_number: this.order_number
					},
					success: (res) => {
						// "订单取消成功"
						// console.log(res)
						// console.log(res.data.code)
						if (res.data.code == 1) {
							uni.showToast({
								icon: 'none',
								title: "订单取消成功"
							});
							this.shuaxin();
						} else {
							uni.showToast({
								icon: 'none',
								title: "订单取消失败"
							})
						}
					}
				})
			},
			//立即付款
			order_payment(e){
				let order_number = e.currentTarget.dataset.order_number || e.target.dataset.order_number;
				var _that=this;
				uni.request({
					url:'https://www.jrtjrt.top/index/goods_order/goods_order_pay',
					data:{
						user_id:this.user_id,
						order_number:order_number
					},
					method:'POST',
					success: (res) => {
						// console.log(res)
						uni.requestPayment({
						    provider: 'wxpay',
						    timeStamp: res.data.data.timeStamp,
						    nonceStr: res.data.data.nonceStr,
						    package: res.data.data.package,
						    signType: res.data.data.signType,
						    paySign: res.data.data.paySign,
						    success: function (res) {
								_that.shuaxin();
						        // console.log('success:' + JSON.stringify(res));
								// uni.navigateTo({
								// 	url:'../paymentsuccessful/paymentsuccessful?allprice='+allprice
								// })
						    },
						    fail: function (err) {
						        // console.log('fail:' + JSON.stringify(err));
								uni.showToast({
									icon:"none",
									title:"未支付"
								})
						    }
						});
					}
				})
				// console.log(order_number)

				
			},
			//查看物流
			btn_logistics(item) {
				console.log(item)
				this.showModalStatus = true;
				// console.log(this.order_list[0].logistics_company)
				// console.log(this.order_list[0].courier_number)
				this.logistics_company=item.logistics_company;
				this.courier_number=item.courier_number;
			},
			//确定收货
			btn_Receiving(e) {
				let order_number = e.currentTarget.dataset.order_number || e.target.dataset.order_number;
				uni.request({
					url: 'https://www.jrtjrt.top/index/goods_order/goods_order_complete',
					data: {
						user_id: this.user_id,
						order_number: order_number
					},
					method: 'POST',
					success: (res) => {
						// console.log(res)
						if (res.data.code == 1) {
							uni.showToast({
								icon: "none",
								title: res.data.message
							});
							this.btn_nav1(2)
						} else {
							uni.showToast({
								icon: "none",
								title: res.data.message
							})
						}
					}
				})
				// console.log(order_number)
			},
			//点击复制快递单号
			btn_copy() {
				uni.setClipboardData({
					data: this.courier_number,
					success: function() {
						console.log('success');
					}
				});
				uni.getClipboardData({
					success: function(res) {
						console.log(res.data);
					}
				});
			},
			btn_close() {
				this.showModalStatus = false;
			},
			btn_box() {
				this.showModalStatus = false;
			}
		}

	}
</script>

<style>
	.order {
		width: 100%;
		background-color: #F1F1F1;
	}

	.order-head {
		display: flex;
		justify-content: space-between;
		font-size: 30upx;
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 60upx;
		background-color: #FFFFFF;
	}

	.order-headbtn {
		display: inline-block;
		width: 25%;
		/* padding-left: 20upx; */
		font-size: 28upx;
		color: #616161;
		line-height: 60upx;
		position: relative;
		text-align: center;
	}

	.order-headbtn .active::after {
		content: "";
		position: absolute;
		width: 78upx;
		height: 0;
		left: 50%;
		margin-left: -39upx;
		bottom: 0;
		border-bottom: 4upx solid #F21C24;
	}

	.order-centre {
		margin-top: 60upx;
		margin-bottom: 60upx;
		/* background-color: #FFFFFF; */
	}

	.order-item {
		margin-top: 10upx;
	}

	/* main */
	.order-main {
		display: flex;
		flex-direction: row;
		/* margin-top: 16upx; */
		padding: 26upx;
		background-color: #FFFFFF;
	}

	.order-main-l {
		width: 156upx;
		height: 156upx;
	}

	.order-main-l>image {
		width: 100%;
		height: 100%;
	}

	.order-main-r {
		padding-left: 26upx;
	}

	.order-main-r-top {
		width: 500upx;
		display: flex;
		flex-direction: column;
	}

	.order-main-r-bottom {
		width: 500upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin-top: 45upx;
	}

	.order-main .order-main-text1 {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		font-size: 26upx;
		color: #333333;
	}

	.order-main .order-main-text2 {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		font-size: 26upx;
		margin-top: 9upx;
		font-size: 24upx;
		color: #999999;
	}

	.order-main .order-main-text3 {
		font-size: 32upx;
		color: #FF3434;
	}

	.order-main .order-main-text4 {
		font-size: 26upx;
		color: #333333;
	}

	.order-lien {
		/* margin-top: 16upx; */
		background-color: #FFFFFF;
		padding: 30upx 30upx 0 30upx;
		display: flex;
		justify-content: space-between;
	}

	.order-lien .order-lien-text1 {
		font-size: 30upx;
		color: #333333;
	}

	.order-lien .order-lien-text2 {
		font-size: 26upx;
		color: #333333;
	}

	.order-fahuo {
		/* width: 720upx; */
		display: flex;
		justify-content: flex-end;
		padding-right: 30upx;
		padding-top: 34upx;
		padding-bottom: 30upx;
		background-color: #FFFFFF;
	}

	.order-fahuo-text1 {
		font-size: 30upx;
		color: #808080;
	}

	.order-fahuo-text2 {
		font-size: 30upx;
		color: #333333;
		margin-left: 26upx;
	}

	/* main1 */
	.order-main1 {
		display: flex;
		flex-direction: column;
		/* margin-top: 10upx; */
		padding: 26upx;
		background-color: #FFFFFF;
	}

	.order-main1-top {
		display: flex;
		flex-direction: row;
	}

	.order-main1-bottom {
		display: flex;
		flex-direction: column;
	}

	.order-main1-top .order-main1-l {
		width: 156upx;
		height: 156upx;
	}

	.order-main1-r {
		display: flex;
		flex-direction: column;
		margin-left: 26upx;
	}

	.order-main1-top .order-main1-l>image {
		width: 100%;
		height: 100%;
	}

	.order-main1-r-top {
		width: 500upx;
		display: flex;
		flex-direction: column;
	}

	.order-main1-r-bottom {
		width: 500upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin-top: 45upx;
	}

	.order-main1-text1 {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		font-size: 26upx;
		color: #333333;
	}

	.order-main1-text2 {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		font-size: 26upx;
		margin-top: 9upx;
		font-size: 24upx;
		color: #999999;
	}

	.order-main1-text3 {
		font-size: 32upx;
		color: #FF3434;
	}

	.order-main1-text4 {
		font-size: 26upx;
		color: #333333;
	}

	.order-lien1 {
		background-color: #FFFFFF;
		padding: 30upx 30upx 0 30upx;
		display: flex;
		justify-content: space-between;
	}

	.order-lien1-text1 {
		font-size: 30upx;
		color: #333333;
	}

	.order-lien1-text2 {
		font-size: 26upx;
		color: #333333;
	}

	.order-fahuo1 {
		display: flex;
		justify-content: flex-end;
		padding-right: 30upx;
		padding-top: 34upx;
		padding-bottom: 30upx;
	}

	.order-fahuo1-text1 {
		font-size: 30upx;
		color: #808080;
	}

	.order-fahuo1-text2 {
		font-size: 30upx;
		color: #333333;
		margin-left: 26upx;
	}

	/* footer */
	.order-centre-footer {
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
		/* padding-top: 30upx; */
		padding-right: 30upx;
		padding-bottom: 20upx;
		background-color: #FFFFFF;
	}

	.order-centre-footer-text1 {
		width: 180upx;
		height: 58upx;
		border-radius: 30upx;
		line-height: 58upx;
		text-align: center;
		font-size: 30upx;
		color: #808080;
		background-color: #FFFFFF;
		border: 2upx solid #808080;
	}

	.order-centre-footer-text2 {
		width: 180upx;
		height: 58upx;
		border-radius: 30upx;
		line-height: 58upx;
		text-align: center;
		font-size: 30upx;
		color: #FF3434;
		background-color: #FFFFFF;
		border: 2upx solid #FF3434;
		margin-left: 30upx;
	}

	.order-centre-footer1 {
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
		/* padding-top: 30upx; */
		padding-right: 30upx;
		padding-bottom: 20upx;
		background-color: #FFFFFF;
	}

	.order-centre-footer1-text1 {
		width: 180upx;
		height: 58upx;
		border-radius: 30upx;
		line-height: 58upx;
		text-align: center;
		font-size: 30upx;
		color: #808080;
		background-color: #FFFFFF;
		border: 2upx solid #808080;
	}

	/* 弹出框 */
	.order-box {
		width: 456upx;
		position: fixed;
		top:300upx;
		left: 76upx;
		z-index: 2000;
		background: #FFFFFF;
		padding: 30upx 72upx 30upx 72upx;
		/* position: relative; */
	}

	.order-box-icon {
		position: absolute;
		top: 28upx;
		right: 28upx;
		width: 44upx;
		height: 44upx;
	}

	.order-box-icon>image {
		width: 100%;
		height: 100%;
	}

	.order-box-item {
		text-align: center;
	}

	.order-box-item1 {
		display: flex;
		flex-direction: column;
		margin-top: 80upx;
		margin-bottom: 80upx;
	}

	.order-box-text1 {
		font-size: 32upx;
		color: #666666;
	}

	.order-box-text2 {
		font-size: 26upx;
		color: #808080;
		margin-top: 20upx;
	}

	.order-box-text3 {
		font-size: 26upx;
		color: #333333;
		margin-top: 20upx;
	}

	.order-box-text4 {
		font-size: 26upx;
		color: #808080;
		margin-top: 20upx;
	}

	.order-box-text5 {
		font-size: 26upx;
		color: #333333;
		margin-top: 20upx;
	}

	.order-box-btn {
		font-size: 26upx;
		color: #FF3434;
		margin-top: 20upx;
	}

	.order-box-btn1 {
		width: 460upx;
		height: 80upx;
		border-radius: 10upx;
		background-color: #FF3434;
		font-size: 30upx;
		color: #FFFFFF;
		text-align: center;
		margin-top: 52upx;
		margin-bottom: 52upx;
	}
	/* 缺 */
	.order-lack{
		width: 300upx;
		height: 300upx;
		margin-top:220upx;
		margin-left: 280upx;
	}
	.order-lack-img{
		width: 200upx;
		height: 200upx;
	}
	.order-lack-img>image{
		width: 100%;
		height: 100%;
	}
	.order-lack>text{
		font-size: 26upx;
		color: #808080;
		text-align: center;
		padding-left: 18upx;
		padding-top: 18upx;
	}
</style>
