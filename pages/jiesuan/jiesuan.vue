<template>
	<view class="jiesuan">
		<view class="jiesuan-head">
			<view class="jiesuan-head-item" @click="addr" v-if="isadds==0">
				<text>请选择收货地址</text>
				<view class="jiesuan-head-icon">
					<image src="../../static/img/icon-dl.png"></image>
				</view>
			</view>
			<view class="jiesuan-head-item1" @click="addr" v-if="isadds==1">
				<view class="jiesuan-head-left">
					<view class="jiesuan-head-left-top">
						<text class="jiesuan-text1">收货人：{{user_address.name}}</text>
						<text class="jiesuan-text2">电话：{{user_address.phone}}</text>
					</view>
					<view class="jiesuan-head-left-bottom">
						<text class="jiesuan-text3">收货地址：{{user_address.address}}</text>
					</view>
				</view>
				<view class="jiesuan-head-right">
					<image src="../../static/img/icon-dl.png"></image>
				</view>
			</view>
		</view>
		<view class="jiesuan-main" v-for="(item,index) in car_list" :key="index" v-if="ask==0">
			<view class="jiesuan-main-l">
				<image :src="item.pic"></image>
			</view>
			<view class="jiesuan-main-r">
				<view class="jiesuan-main-r-top">
					<text class="jiesuan-main-text1">{{item.goods_name}}</text>
					<text class="jiesuan-main-text2">{{item.specs_name}}</text>
				</view>
				<view class="jiesuan-main-r-bottom">
					<text class="jiesuan-main-text3">¥ {{level?item.member_money:item.money}}</text>
					<text class="jiesuan-main-text4">x{{item.num}}</text>
				</view>
			</view>
		</view>
		<view class="jiesuan-main" v-if="ask==1">
			<view class="jiesuan-main-l">
				<image :src="car_list.pic"></image>
			</view>
			<view class="jiesuan-main-r">
				<view class="jiesuan-main-r-top">
					<text class="jiesuan-main-text1">{{car_list.name}}</text>
					<text class="jiesuan-main-text2">{{specs_name}}</text>
				</view>
				<view class="jiesuan-main-r-bottom">
					<text class="jiesuan-main-text3">¥ {{level?member_money:car_list.money}}</text>
					<text class="jiesuan-main-text4">x{{num}}</text>
				</view>
			</view>
		</view>
		<view class="jiesuan-lien">
			<text class="jiesuan-lien-text1">配送费</text>
			<text class="jiesuan-lien-text2">{{afare}}元</text>
		</view>
		<view class="jiesuan-bottom">
			<text>买家留言</text>
			<input type='text' v-model="words" placeholder="请填写备注" />
		</view>
		<view class="jiesuan-footer">
			<view class="jiesuan-footer-left">总计：￥{{allprice}}</view>
			<view class="jiesuan-footer-rigth" @click="btn_submit">提交</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				car_list: '',
				user_address: '',
				isShow: false,
				words: '',
				//配送费
				afare: 0,
				//总价	
				allprice: 0,
				ask: -1,
				num:1,
				specs_id:0,
				specs_name:'',
				isadds:'',
				level:'',
				member_money:''
			}
		},
		onLoad(options) {
			
			this.user_id = uni.getStorageSync('user_id');
			// console.log(uni.getStorageSync("user_id"),this.user_id)
			var ask = options.ask,
				num = options.num,
				specs_id = options.specs_id,
				id = options.id;
			// console.log(ask)
			this.ask = options.ask;
			this.num=options.num;
			this.specs_id = options.specs_id;
			this.goods_id=options.id;
			this.member_money=options.member_money;
			// console.log(this.member_money)

			var _that = this;
			if (ask == 0) {
				this.arr = JSON.parse(uni.getStorageSync('cartId'));
				let str = "";
				for (let i = 0; i < this.arr.length; i++) {
					str = str + this.arr[i] + ","
				}
				uni.request({
					url: 'https://www.jrtjrt.top/index/shopping_cart/shopping_cart_lst_d',
					data: {
						user_id: this.user_id,
						id: str.substring(0, str.length - 1)
					},
					methods: "post",
					success: (res) => {
						_that.car_list = res.data.data;
						let afare = 0;
						let topric = 0;
						// for (let i = 0; i < _that.car_list.length; i++) {
						// 	afare += Number(this.car_list[i].fare);
						// 	// var afare += this.fare
						// 	topric = topric + this.car_list[i].num * Number(this.car_list[i].money)
						
						// }
						// // console.log(topric)
						// _that.afare = afare;
						// _that.allprice = afare + topric;
						uni.request({
							url: 'https://www.jrtjrt.top/index/user/user_lst_d',
							data: {
								user_id: this.user_id
							},
							method: 'POST',
							success: (res) => {
								// console.log(res.data.data.level);
								_that.level = res.data.data.level;
								// console.log(_that.level)
								if(_that.level==0){
									for (let i = 0; i < _that.car_list.length; i++) {
										afare += Number(this.car_list[i].fare);
										// var afare += this.fare
										topric = topric + this.car_list[i].num * Number(this.car_list[i].money)
									
									}
									// console.log(topric)
									_that.afare = afare;
									_that.allprice = afare + topric;
								}else if(_that.level==1){
									for (let i = 0; i < _that.car_list.length; i++) {
										afare += Number(this.car_list[i].fare);
										// var afare += this.fare
										console.log(afare)
										topric = topric + this.car_list[i].num * Number(this.car_list[i].member_money)
									
									}
									// console.log(topric)
									_that.afare = afare;
									_that.allprice = afare + topric;
								}else if(_that.level==2){
									for (let i = 0; i < _that.car_list.length; i++) {
										afare += Number(this.car_list[i].fare);
										// var afare += this.fare
										console.log(afare)
										topric = topric + this.car_list[i].num * Number(this.car_list[i].member_money)
									
									}
									// console.log(topric)
									_that.afare = afare;
									_that.allprice = afare + topric;
								}else if(_that.level==3){
									for (let i = 0; i < _that.car_list.length; i++) {
										afare += Number(this.car_list[i].fare);
										// var afare += this.fare
										console.log(afare)
										topric = topric + this.car_list[i].num * Number(this.car_list[i].member_money)
									
									}
									// console.log(topric)
									_that.afare = afare;
									_that.allprice = afare + topric;
								}
							}
						})
						

					}
				})
			} else if (ask == 1) {
				
				uni.request({
					url: 'https://www.jrtjrt.top/index/goods/goods_lst_ds',
					data: {
						user_id: this.user_id,
						id: id
					},
					methods: "post",
					success: (res) => {
						_that.car_list = res.data.data;
						// console.log(res.data)
						let afare = 0;
						let topric = 0;
						// afare=_that.car_list.fare;
						// topric=_that.num * Number(this.car_list.money)
						// _that.afare = afare;
						// _that.allprice = Number(afare) + Number(topric);
						uni.request({
							url: 'https://www.jrtjrt.top/index/user/user_lst_d',
							data: {
								user_id: this.user_id
							},
							method: 'POST',
							success: (res) => {
								// console.log(res.data.data.level);
								_that.level = res.data.data.level;
								// console.log(_that.level)
								if(_that.level==0){
									afare=_that.car_list.fare;
									topric=_that.num * Number(this.car_list.money)
									_that.afare = afare;
									_that.allprice = Number(afare) + Number(topric);
								}else if(_that.level==1){
									afare=_that.car_list.fare;
									topric=_that.num * Number(_that.member_money)
									_that.afare = afare;
									// console.log(afare)
									_that.allprice = Number(afare) + Number(topric);
								}else if(_that.level==2){
									afare=_that.car_list.fare;
									topric=_that.num * Number(_that.member_money)
									_that.afare = afare;
									// console.log(afare)
									_that.allprice = Number(afare) + Number(topric);
								}else if(_that.level==3){
									// console.log(1)
									afare=_that.car_list.fare;
									topric=_that.num * Number(_that.member_money)
									_that.afare = afare;
									// console.log(afare)
									_that.allprice = Number(afare) + Number(topric);
								}
							}
						})
						uni.request({
							url:'https://www.jrtjrt.top/index/specs/specs_lst_d',
							data:{
								id:_that.specs_id
							},
							method:'POST',
							success: (res) => {
								// console.log(res.data.data.name)
								_that.specs_name=res.data.data.name
							}
						})
					}
				})
			}

		},
		onShow() {
			this.user_address = uni.getStorageSync('user_address');
			// console.log(uni.getStorageSync('cartId'));
			if(this.user_address.length==0){
				this.isadds=0
			}else{
				this.isadds=1
			}
			uni.request({
				url: 'https://www.jrtjrt.top/index/user/user_lst_d',
				data: {
					user_id: this.user_id
				},
				method: 'POST',
				success: (res) => {
					// console.log(res.data.data.level);
					this.level = res.data.data.level;
					// console.log(_that.level)
					if(this.level==0){
						
					}else if(this.level==1){
						
					}else if(this.level==2){
						
					}else if(this.level==3){
					
					}
				}
			})

		},
		methods: {
			addr() {
				uni.navigateTo({
					url: '../shaddress/shaddress'
				})
			},
			btn_submit() {
				let car_arr = [];
				
				let allprice=this.allprice;
				// console.log(allprice)
				for (let i = 0; i < this.car_list.length; i++) {
					car_arr.push(this.car_list[i].id)

				}
				let str1 = "";
				for (let i = 0; i < car_arr.length; i++) {
					str1 = str1 + car_arr[i] + ","
				}
				if (this.ask == 0) {
					uni.request({
						url: 'https://www.jrtjrt.top/index/goods_order/goods_order_adds',
						data: {
							user_id: this.user_id,
							goods_id: str1.substring(0, str1.length - 1),
							address_id: this.user_address.id,
							content: this.words
						},
						method: 'POST',
						success: (res) => {
							// console.log(res.data.data)
							if(res.data.code==1){
								uni.requestPayment({
								    provider: 'wxpay',
								    timeStamp: res.data.data.timeStamp,
								    nonceStr: res.data.data.nonceStr,
								    package: res.data.data.package,
								    signType: res.data.data.signType,
								    paySign: res.data.data.paySign,
								    success: function (res) {
								        // console.log('success:' + JSON.stringify(res));
										uni.navigateTo({
											url:'../paymentsuccessful/paymentsuccessful?allprice='+allprice
										})
								    },
								    fail: function (err) {
								        // console.log('fail:' + JSON.stringify(err));
										uni.showToast({
											icon:"none",
											title:"未支付"
										})
										uni.navigateTo({
											url:'../order/order?type=0'
										})
								    }
								});
							}else if(res.data.code==0){
								uni.showToast({
									icon:'none',
									title:res.data.message
								})
							}

						}
					})
				} else if (this.ask == 1) {
					// console.log(this.num);
					// console.log(this.specs_id);
					// console.log(this.goods_id);
					// console.log(this.user_address.id);
					// console.log(this.words);
					uni.request({
						url: 'https://www.jrtjrt.top/index/goods_order/goods_order_add',
						data: {
							user_id: this.user_id,
							goods_id: this.goods_id,
							specs_id:this.specs_id,
							num:this.num,
							address_id: this.user_address.id,
							content: this.words
						},
						method: 'POST',
						success: (res) => {
							// console.log(res.data.code)
							if(res.data.code==1){
								// console.log(res.data.code)
								uni.requestPayment({
								    provider: 'wxpay',
								    timeStamp: res.data.data.timeStamp,
								    nonceStr: res.data.data.nonceStr,
								    package: res.data.data.package,
								    signType: res.data.data.signType,
								    paySign: res.data.data.paySign,
								    success: function (res) {
								        // console.log('success:' + JSON.stringify(res));
										uni.navigateTo({
											url:'../paymentsuccessful/paymentsuccessful?allprice='+allprice
										})
								    },
								    fail: function (err) {
								        // console.log('fail:' + JSON.stringify(err));
										uni.showToast({
											icon:"none",
											title:"未支付"
										})
										uni.navigateTo({
											url:'../order/order?type=0'
										})
								    }
								});
							}else if(res.data.code==0){
								// console.log(res.data)
									uni.showToast({
										icon:'none',
										title:res.data.message
									})
								// if(res.data.message){
								// 	uni.showToast({
								// 		icon:'none',
								// 		title:res.data.message
								// 	})
								// }else{
								// 	uni.showToast({
								// 		icon:'none',
								// 		title:res.data
								// 	})
								// }
																
							}

							// console.log(res)
						}
					})
				}

			}
		}
	}
</script>

<style>
	.jiesuan {
		width: 100%;
		background-color: #F5F5F5;
	}

	.jiesuan-head {
		/* width: 100%; */
		background-color: #FFFFFF;
		padding: 30upx 30upx 30upx 30upx;
		margin-top: 2upx;
	}

	.jiesuan-head-item {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}

	.jiesuan-head-item>text {
		font-size: 30upx;
		color: #333333;
	}

	.jiesuan-head-icon>image {
		width: 22upx;
		height: 40upx;
	}

	.jiesuan-head-item1 {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}

	.jiesuan-text1 {
		font-size: 30upx;
		color: #333333;

	}

	.jiesuan-text2 {
		font-size: 30upx;
		color: #333333;
		margin-left: 60upx;
	}

	.jiesuan-text3 {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		font-size: 30upx;
		color: #333333;
	}

	.jiesuan-head-left {
		width: 652upx;
	}

	.jiesuan-head-left-top {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		width: 100%;
	}

	.jiesuan-head-left-bottom {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		width: 100%;
		margin-top: 28upx;
	}

	.jiesuan-head-right {
		margin-top: 30upx;
	}

	.jiesuan-head-right>image {
		width: 22upx;
		height: 40upx;
	}

	.jiesuan-main {
		display: flex;
		flex-direction: row;
		margin-top: 16upx;
		padding: 26upx;
		background-color: #FFFFFF;
	}

	.jiesuan-main-l {
		width: 156upx;
		height: 156upx;
	}

	.jiesuan-main-l>image {
		width: 100%;
		height: 100%;
	}

	.jiesuan-main-r {
		padding-left: 26upx;
	}

	.jiesuan-main-r-top {
		width: 500upx;
		display: flex;
		flex-direction: column;
	}

	.jiesuan-main-r-bottom {
		width: 500upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin-top: 45upx;
	}

	.jiesuan-main .jiesuan-main-text1 {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		font-size: 26upx;
		color: #333333;
	}

	.jiesuan-main .jiesuan-main-text2 {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		font-size: 26upx;
		margin-top: 9upx;
		font-size: 24upx;
		color: #999999;
	}

	.jiesuan-main .jiesuan-main-text3 {
		font-size: 32upx;
		color: #FF3434;
	}

	.jiesuan-main .jiesuan-main-text4 {
		font-size: 26upx;
		color: #333333;
	}

	.jiesuan-lien {
		margin-top: 16upx;
		background-color: #FFFFFF;
		padding: 30upx;
		display: flex;
		justify-content: space-between;
	}

	.jiesuan-lien .jiesuan-lien-text1 {
		font-size: 30upx;
		color: #333333;
	}

	.jiesuan-lien .jiesuan-lien-text2 {
		font-size: 26upx;
		color: #FF3434;
	}

	.jiesuan-bottom {
		background-color: #FFFFFF;
		display: flex;
		flex-direction: row;
		margin-top: 16upx;
		padding: 30upx;
		margin-bottom: 102upx;
	}

	.jiesuan-bottom>text {
		font-size: 30upx;
		color: #333333;
	}

	.jiesuan-bottom>input {
		width: 78%;
		padding-left: 28upx;
		font-size: 30upx;
		color: #616161;
	}

	.jiesuan-footer {
		width: 100%;
		height: 100upx;
		position: fixed;
		left: 0;
		bottom: 0;
		background-color: #FFFFFF;
		display: flex;
		flex-direction: row;
		z-index: 999;

	}

	.jiesuan-footer-left {
		width: 470upx;
		height: 100upx;
		line-height: 100upx;
		/* text-align: center; */
		color: #FF3434;
		font-size: 30upx;
		padding-left: 30upx;
	}

	.jiesuan-footer-rigth {
		width: 250upx;
		height: 100upx;
		line-height: 100upx;
		text-align: center;
		color: #FFFFFF;
		background-color: #FF3434;
		font-size: 30upx;
	}
</style>
