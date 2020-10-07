<template>
	<view class="gocart" v-if="showF">
		<view class="gocart-main">
			<checkbox-group class="check" name="check" @change="changeitem" :value="checkList">
				<view class="golist" v-for="(item,index) in detriment" :key="index" v-if="isShow==1">
					<!-- 单选 -->
					<view class="golist-l">
						<checkbox :value="index" :checked="item.flag"></checkbox>

					</view>
					<view class="golist-t" :data-id="item.goods_id" @click="btn_detaxx">
						<image :src="item.pic"></image>
					</view>
					<view class="golist-r">
						<view class="golist-r1">
							<view class="golist-r1-l">
								<view class="golist-r-text1">{{item.goods_name}}</view>
								<view class="golist-r-text2">￥{{item.money}}</view>
							</view>
							<image src="../../static/img/shanchu.png" @click="del(item,index)"></image>
						</view>
						<view class="golist-r2">
							<label class="minute" @click="btn_minute(index)">-</label>
							<input class="count" type="text" disabled=true v-model="item.num" />
							<label class="add" @click="btn_add(index)">+</label>
						</view>
					</view>
				</view>
			</checkbox-group>
			<view class="gocart-lack" v-if="isShow==0">
				<view class="gocart-lack-img">
					<image src="../../static/img/gocart/gocart-lack.png"></image>
				</view>
				<text>购物车是空的哦</text>
			</view>
			<!-- 底部 -->
			<view class="gocart-footer">
				<!-- 全选按钮 -->
				<view class="quanxuan">
					<checkbox-group class="allBtn" @change="allchange">
						<checkbox value="cg" :checked="allchecked" />全选
					</checkbox-group>
				</view>
				<view class="zongjia">
					<text>总计：<text>￥{{allprice}}</text></text>
				</view>
				<view class="jiesuan" @click="jiesuan(0)">
					<text>结算</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				detriment: [],
				// flag: false,
				// 全选
				allchecked: false,
				// 总价	
				allprice: 0,
				//总数   
				num: 0,
				showF: false,
				isShow: ''
			}
		},
		computed: {
			// 计算选中商品的总价

		},
		onShow() {
			this.user_id = uni.getStorageSync('user_id');
			try {
				const value = uni.getStorageSync('userInfo');
				if (value) {
					// console.log(value);
					this.showF = true;

				} else {
					uni.switchTab({
						url: '/pages/user/user'
					});
					uni.showToast({
						icon: "none",
						title: "请微信授权"
					})
				}
			} catch (e) {
				// console.log(11)
			}
			this.btn()
		},
		onLoad() {
			var totalPrice = 0;
			var detriment = this.detriment;
			for (let i = 0; i < this.detriment.length; i++) {
				if (this.detriment[i].flag) {
					totalPrice = totalPrice + this.detriment[i].num * Number(this.detriment[i].money)

				}
			}
			// for (var i = 0; i < detriment.length; i++) {
			// 	if (flag == true) {
			// 		// console.log(11);
			// 		totalPrice += detriment[i].num * Number(detriment[i].money)
			// 	}
			// }
			// console.log(totalPrice);
			this.allprice = totalPrice.toFixed(2);
			// console.log(this.allprice)

			// console.log(this.user_id,uni.getStorageSync("user_id"))
		},
		//分享
		onShareAppMessage() {

		},
		methods: {
			btn() {
				uni.request({
					url: 'https://www.jrtjrt.top/index/shopping_cart/shopping_cart_lst',
					data: {
						user_id: this.user_id
					},
					methods: "post",
					success: (res) => {

						// console.log(res.data.data.data);
						this.detriment = res.data.data.data.map(cur => {
							cur.flag = false;
							return cur;

						});
						if (res.data.data.data == '') {
							this.isShow = 0;
							// console.log(1)
							// console.log(this.detriment)
						} else {
							this.isShow = 1;
							// console.log(2)
							// console.log(this.detriment)
						}
						this.allchecked = false;
						this.hh()
					}
				})
			},

			//总价总数量方法   方便调用   再次多写了一次
			hh: function() {
				let totalPrice = 0;
				for (let i = 0; i < this.detriment.length; i++) {
					if (this.detriment[i].flag) {
						totalPrice = totalPrice + this.detriment[i].num * Number(this.detriment[i].money)

					}
				}
				this.allprice = totalPrice.toFixed(2);
			},
			btn_detaxx(e) {
				let id = e.currentTarget.dataset.id || e.target.dataset.id;
				uni.navigateTo({
					url: '/pages/details/details?id=' + id
				})
			},
			//减去
			btn_minute: function(index) {
				var detriment = this.detriment;
				var num = detriment[index].num;
				var id = detriment[index].id;
				if (num > 1) {
					num = num - 1;
					detriment[index].num = num;
				} else {
					// alert('再减就没有商品啦')
				}
				this.hh();
				var _that = this;

				// console.log(id)
				// console.log(this.user_id)
				uni.request({
					url: 'https://www.jrtjrt.top/index/shopping_cart/shopping_cart_num_upd',
					data: {
						user_id: this.user_id,
						id: id,
						state: 2
					},
					methods: "post",
					success: (res) => {
						// console.log(res)
					}
				})
			},
			//添加
			btn_add: function(index) {
				var detriment = this.detriment;
				var num = parseInt(detriment[index].num);
				var id = detriment[index].id;
				if (num < 100) {
					num = num + 1;
					detriment[index].num = num;
				} else {
					alert('商品仅限购买10件')
				}
				this.hh();
				uni.request({
					url: 'https://www.jrtjrt.top/index/shopping_cart/shopping_cart_num_upd',
					data: {
						user_id: this.user_id,
						id: id,
						state: 1
					},
					methods: "post",
					success: (res) => {
						// console.log(res)
					}
				})
			},
			//商品的删除
			del(item, index) {
				// console.log(item.id)
				uni.request({
					url: 'https://www.jrtjrt.top/index/shopping_cart/specs_sort_del',
					data: {
						specs_sort_id: item.id
					},
					method: 'POST',
					success: (res) => {
						// console.log(res.data.code);
						if (res.data.code == 1) {
							this.btn()
						}
					}
				})

			},
			//商品结算
			jiesuan(ask) {
				let arr = [];
				for (let i = 0; i < this.detriment.length; i++) {
					// console.log(this.detriment[i])
					if (this.detriment[i].flag) {
						arr.push(this.detriment[i].id)
					}
				}
				uni.setStorageSync("cartId", JSON.stringify(arr))
				if (arr.length == 0) {
					uni.showToast({
						icon: "none",
						title: "未选中商品"
					})
				} else {
					uni.navigateTo({
						url: '../jiesuan/jiesuan?ask=' + ask
					})
				}

			},
			// 单选
			changeitem(e) {
				// console.log(e)
				if (e.detail.value.length == this.detriment.length) {
					this.allchecked = true;
					this.checkList = e.detail.value;
					for (let i = 0; i < this.detriment.length; i++) {
						this.detriment[i].flag = false;
					}
					this.checkList.forEach((cur, index) => {
						this.detriment[cur].flag = true;
					})
					// this.hh()
				} else {
					this.allchecked = false;
					this.checkList = e.detail.value;
					for (let i = 0; i < this.detriment.length; i++) {
						this.detriment[i].flag = false;
					}
					this.checkList.forEach((cur, index) => {
						this.detriment[cur].flag = true;
					})
					// this.hh()
				}
				this.hh()
				// this.flag = !this.flag;
				// // console.log(this.flag)
				// if (this.flag) {
				// 	// let test = this.detriment.every(item => {
				// 	// 	return flag === true
				// 	// })
				// 	// console.log(this.flag)
				// 	// this.flag=true;
				// 	this.hh()
				// 	if (this.flag) {
				// 		this.allchecked = true
				// 	} else {
				// 		this.allchecked = false
				// 	}
				// } else {					
				// 	this.allchecked = false
				// 	this.hh()
				// }

			},
			//全选
			allchange() {
				this.allchecked = !this.allchecked
				// 如果 allcheckked 为真  全选被选中时，那么设置每一项都被选中
				if (this.allchecked) {
					for (var i = 0; i < this.detriment.length; i++) {
						this.detriment[i].flag = true;
					}
				} else {
					for (var i = 0; i < this.detriment.length; i++) {
						this.detriment[i].flag = false;
					}
				}
				this.hh()
			}

		}
	}
</script>

<style>
	/* checkbox样式修改 */
	checkbox .wx-checkbox-input {
		border-radius: 50%;
		/* 		background-color: #ff4500;
		color: #FFFFFF !important; */
	}

	checkbox .wx-checkbox-input.wx-checkbox-input-checked {
		border: 2upx solid #ff4500;
		background: #ff4500;
		color: #FFFFFF !important;
	}

	checkbox .wx-checkbox-input.wx-checkbox-input-checked::before {
		font-size: 38upx;
	}

	.gocart {
		width: 100%;
		margin-bottom: 80upx;
	}

	.gocart-main {
		width: 100%;
		/* position: relative; */
	}

	.golist {
		padding: 30upx 0 30upx 0;
		display: flex;
		flex-direction: row;
		border-bottom: 2upx solid #F1F1F1;
	}

	.golist-l {
		width: 40upx;
		height: 40upx;
		margin-top: 100upx;
		margin-left: 30upx;
	}

	.golist-t {
		margin-left: 50upx;
	}

	.golist-t image {
		width: 250upx;
		height: 250upx;
		margin: 0 auto;
	}

	.golist-r {
		width: 100%;
		margin-left: 20upx;
	}

	.golist-r1 {
		display: flex;
		flex-direction: row;
		margin-top: 30upx;
	}

	.golist-r1-l {
		width: 250upx;
		display: flex;
		flex-direction: column;
	}

	.golist-r1 image {
		width: 32upx;
		height: 32upx;
		margin-top: 6upx;
		margin-left: 30upx;
	}

	.golist-r-text1 {
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		color: #333333;
		font-size: 28upx;
	}

	.golist-r-text2 {
		color: #FF3939;
		font-size: 30upx;
		margin-top: 10upx;
	}

	.golist-r2 {
		width: 210upx;
		height: 60upx;
		margin-top: 75upx;
		border: 2upx solid #E5E5E5;
		display: flex;
		flex-direction: row;
	}

	.minute {
		width: 70upx;
		text-align: center;
		line-height: 56upx;
		font-size: 36upx;
		height: 60upx;
		border-right: 2upx solid #E5E5E5;
	}

	.count {
		width: 70upx;
		height: 60upx;
		/* height: 36upx; */
		/* border-top: 2upx solid #E5E5E5; */
		/* border-bottom: 2upx solid #E5E5E5; */
		text-align: center;
	}

	.add {
		width: 70upx;
		border-left: 2upx solid #E5E5E5;
		line-height: 56upx;
		text-align: center;
		font-size: 36upx;
		height: 60upx;
	}

	.gocart-footer {
		/* 		position: absolute;
		bottom:0; */
		width: 100%;
		height: 96upx;
		display: flex;
		justify-content: space-between;
		text-align: center;
		padding: 20upx;
		box-sizing: border-box;
		position: fixed;
		bottom: 0;
		background-color: #FFFFFF;
		z-index: 9999;

	}

	.quanxuan {
		width: 140upx;
		height: 96upx;
		text-align: center;
		overflow: hidden;
		white-space: nowrap;
	}

	.allBtn {
		/* 		width: 40upx;
		height: 40upx;
		line-height: 40upx; */
	}

	.zongjia {
		margin-left: 120upx;
		font-size: 30upx;
		color: #FF3939;
		line-height: 60upx;
		white-space: nowrap;
		/* padding-left: 30upx; */
	}

	.jiesuan {
		width: 190upx;
		height: 60upx;
		line-height: 60upx;
		text-align: center;
		background-color: #FF3939;
		color: #FFFFFF;
		border-radius: 30upx;
		margin-left: 30upx;
	}

	/* 缺 */
	.gocart-lack {
		width: 300upx;
		height: 300upx;
		margin-top: 220upx;
		margin-left: 280upx;
	}

	.gocart-lack-img {
		width: 200upx;
		height: 200upx;
	}

	.gocart-lack-img>image {
		width: 100%;
		height: 100%;
	}

	.gocart-lack>text {
		font-size: 26upx;
		color: #808080;
		text-align: center;
		padding-left: 18upx;
		padding-top: 18upx;
	}
</style>
