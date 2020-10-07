<template>
	<view class="user">
		<view class="user_top">
			<view class="user_info_box" @click="bindLogout()">
				<image :src="uesr_Img"></image>
			</view>
			<button class="user_info_btn" type="default" open-type="getUserInfo" @getuserinfo="bindLogout"></button>
			<view class="user_text">
				<text class="user_text1">{{name}}</text>
				<text class="user_text3" v-if="istext1==0">请点击登录</text>
				<text class="user_text2">{{istext}}</text>
			</view>
		</view>
		<view class="user_main">
			<view class="user_top_t">
				<text class="user_top_t_text1">我的订单</text>
				<view class="user_top_t_r" @click="order(0)">
					<text class="user_top_t_text2">查看全部</text>
					<image src="../../static/img/my/icon-r.png"></image>
				</view>

			</view>
			<view class="user_top_f">
				<view class="user_item" @click="btn_order1(0)">
					<view class="user_item_icon">
						<image src="../../static/img/my/qianbao.png"></image>
						<text v-if="shu1.length">{{shu1.length}}</text>
						<!-- <text v-if="!shu1.length">{{0}}</text> -->
					</view>
					<view class="user_item_text">待付款 </view>
				</view>
				<view class="user_item" @click="btn_order1(1)">
					<view class="user_item_icon">
						<image src="../../static/img/my/fah.png"></image>
						<text v-if="shu2.length">{{shu2.length}}</text>
						
					</view>
					<view class="user_item_text">待发货</view>
				</view>
				<view class="user_item" @click="btn_order1(2)">
					<view class="user_item_icon">
						<image src="../../static/img/my/shoh.png"></image>
						<text v-if="shu3.length">{{shu3.length}}</text>
						<!-- <text v-if="!shu3.length">{{0}}</text> -->
					</view>
					<view class="user_item_text">待收货</view>
				</view>
				<view class="user_item" @click="btn_order1(3)">
					<view class="user_item_icon">
						<image src="../../static/img/my/fah.png"></image>
						<text v-if="shu4.length">{{shu4.length}}</text>
						<!-- <text v-if="!shu4.length">{{0}}</text> -->
					</view>
					<view class="user_item_text">已完成</view>
				</view>
			</view>
		</view>
		<view class="user_footer">
			<view class="user_footer_t">我的服务</view>
			<view class="user_footer_f">
				<!-- 				<view class="user_item1" @click="btn_hhr">
					<view class="user_item_icon1">
						<image src="../../static/img/my/hhr.png"></image>
					</view>
					<view class="user_item_text1">vip</view>
				</view> -->
				<view class="user_item1" @click="user_dongshi">
					<view class="user_item_icon1">
						<image src="../../static/img/my/qgds.png"></image>
					</view>
					<view class="user_item_text1">全国董事</view>
				</view>
				<view class="user_item1" @click="btn_fenxian">
					<view class="user_item_icon1">
						<image src="../../static/img/my/fxzc.png"></image>
					</view>
					<view class="user_item_text1">分销中心</view>
				</view>
				<view class="user_item1" @click="btn_shaddress">
					<view class="user_item_icon1">
						<image src="../../static/img/my/shdz.png"></image>
					</view>
					<view class="user_item_text1">收货地址</view>
				</view>
				<view class="user_item1">
					<view class="user_item_icon1">
						<image src="../../static/img/my/lxkf.png"></image>
					</view>
					<view class="user_item_text1">
						联系客服
						<button class="user-btnkf" open-type="contact"></button>
					</view>
				</view>
			</view>
		</view>
	</view>
	<!-- 	<view class="content">
		<view class="btn-row">
			<button v-if="!hasLogin" type="primary" class="primary" @tap="bindLogin">登录</button>
			<button v-if="hasLogin" type="default" @tap="bindLogout">退出登录</button>
		</view>
	</view> -->
</template>

<script>
	import {
		mapState,
		mapMutations
	} from 'vuex'

	export default {
		data() {
			return {
				name: '',
				uesr_Img: '../../static/img/my/touxiang1.png',
				judgement: false, //判断是否微信授权
				level: '',
				istext: '',
				istext1: 0,
				shu1: '',
				shu2:'',
				shu3:'',
				shu4:''
			}
		},
		onLoad() {
			// this.bindLogout()
			try {
				const value1 = uni.getStorageSync('user_id');
				const value = uni.getStorageSync('userInfo');

				if (value1) {
					var uesr_data = JSON.parse(value);
					this.name = uesr_data.nickName;
					this.uesr_Img = uesr_data.avatarUrl;
					this.judgement = true;
					// console.log(JSON.parse(value))					
				}
			} catch (e) {}
		},
		onShow() {
			this.user_id = uni.getStorageSync('user_id');
			this.fx_id = uni.getStorageSync('fx_id');
			// console.log(this.user_id);
			this.user_shxin();
			uni.request({
				url: 'https://www.jrtjrt.top/index/goods_order/goods_order_lst',
				data: {
					user_id: this.user_id,
					type: 0
				},
				method: 'POST',
				success: (res) => {
					// console.log(res.data.data.data)
					this.shu1 = res.data.data.data;
					// console.log(this.order_list);

				}
			})
			uni.request({
				url: 'https://www.jrtjrt.top/index/goods_order/goods_order_lst',
				data: {
					user_id: this.user_id,
					type: 1
				},
				method: 'POST',
				success: (res) => {
					// console.log(res.data.data.data)
					this.shu2 = res.data.data.data;

					// console.log(this.order_list);

				}
			})
			uni.request({
				url: 'https://www.jrtjrt.top/index/goods_order/goods_order_lst',
				data: {
					user_id: this.user_id,
					type: 2
				},
				method: 'POST',
				success: (res) => {
					// console.log(res.data.data.data)
					this.shu3 = res.data.data.data;
					// console.log(this.order_list);

				}
			})
			uni.request({
				url: 'https://www.jrtjrt.top/index/goods_order/goods_order_lst',
				data: {
					user_id: this.user_id,
					type: 3
				},
				method: 'POST',
				success: (res) => {
					// console.log(res.data.data.data)
					this.shu4 = res.data.data.data;
					// console.log(this.order_list);

				}
			})

		},
		//分享
		onShareAppMessage() {

		},
		computed: {
			...mapState(['hasLogin', 'forcedLogin'])
		},
		methods: {
			...mapMutations(['logout']),
			bindLogin() {
				uni.navigateTo({
					url: '../login/login',
				});
			},
			//刷新
			user_shxin() {
				var _that = this
				if (this.user_id) {
					this.istext1 = 1
					// console.log(this.istext1)
				}
				uni.request({
					url: 'https://www.jrtjrt.top/index/user/user_lst_d',
					data: {
						user_id: this.user_id
					},
					method: 'POST',
					success: (res) => {
						// console.log(res.data.data);
						this.level = res.data.data.level;

						// console.log(this.level)
						if (this.level == 0) {
							this.istext = "普通会员"

						} else if (this.level == 1) {

							this.istext = "合伙人"
						} else if (this.level == 2) {

							this.istext = "分销商"
						} else if (this.level == 3) {
							this.istext = "全国董事"
						}
					}
				});
			},
			bindLogout() {
				if (this.judgement == false) {
					var _this = this;
					uni.login({
						provider: 'weixin',
						success: function(loginRes) {
							// console.log(loginRes);
							var code = loginRes.code;
							// console.log(code)
							uni.getUserInfo({
								provider: 'weixin',
								success: function(infoRes) {
									// console.log(code);
									// console.log(infoRes)
									uni.request({
										url: 'https://www.jrtjrt.top/index/login/login',
										data: {
											code: code,
											nickName: infoRes.userInfo.nickName,
											avatarUrl: infoRes.userInfo.avatarUrl,
											city: infoRes.userInfo.city,
											country: infoRes.userInfo.country,
											uid: _this.fx_id
										},
										methods: "post",
										success: (res) => {
											// console.log(res.data.data.id);
											uni.setStorageSync("user_id", res.data.data.id);
											if (res.data.data.id) {
												_this.istext1 = 1
												// console.log(_this.istext1)
											}
											// var data = res.data.data.data;
											// _that.shangpinList = data;
											uni.request({
												url: 'https://www.jrtjrt.top/index/user/user_lst_d',
												data: {
													user_id: res.data.data.id
												},
												method: 'POST',
												success: (res) => {
													// console.log(res.data.data.level);
													_this.level = res.data.data.level;
													// console.log(_this.level)

													if (_this.level == 0) {
														_this.istext = "普通会员"

													} else if (_this.level == 1) {

														_this.istext = "合伙人"
													} else if (_this.level == 2) {

														_this.istext = "分销商"
													} else if (_this.level == 3) {
														_this.istext = "全国董事"
													}
												}
											});
										}
									});
									// console.log(JSON.stringify(infoRes.userInfo));
									var uesr_data = infoRes.userInfo;
									_this.name = uesr_data.nickName;
									_this.uesr_Img = uesr_data.avatarUrl;
									_this.judgement = true;
									uni.setStorageSync("userInfo", JSON.stringify(uesr_data));
									// this.user_shxin();
								},
								fail(err) {
									console.log(err)
								}
							})
						},
						fail(err) {
							console.log(err)
						}
					})
				}

			},
			//我的订单
			order(type) {
				uni.navigateTo({
					url: '../order/order?type=' + type
				})
			},
			//合伙人
			btn_hhr() {
				const user_id1 = uni.getStorageSync('user_id');
				if (user_id1) {
					uni.navigateTo({
						url: '/pages/partner/partner'
					})
				} else {
					uni.showToast({
						icon: "none",
						title: "请你微信授权"
					})
				}
			},
			//全国董事
			user_dongshi() {
				const user_id2 = uni.getStorageSync('user_id');
				if (user_id2) {
					if (this.level == 3) {
						uni.navigateTo({
							url: '../dongshibution/dongshibution'
						})
					} else {
						uni.navigateTo({
							url: '../dongshi/dongshi'
						})
					}
				} else {
					uni.showToast({
						icon: "none",
						title: "请你微信授权"
					})
				}
			},
			//分销中心
			btn_fenxian() {
				const user_id3 = uni.getStorageSync('user_id');
				if (user_id3) {
					if (this.level == 2) {
						uni.navigateTo({
							url: '../distribution/distribution'
						})
					} else if (this.level == 3) {
						uni.navigateTo({
							url: '../distribution/distribution'
						})
					} else if (this.level == 1) {
						uni.navigateTo({
							url: '../distribution/distribution'
						})
					} else {
						uni.navigateTo({
							url: '../partner/partner'
						})
					}
				} else {
					uni.showToast({
						icon: "none",
						title: "请你微信授权"
					})
				}

			},
			btn_order1(type) {
				uni.navigateTo({
					url: '../order/order?type=' + type
				})
			},
			//收货地址
			btn_shaddress() {
				const user_id3 = uni.getStorageSync('user_id');
				if (user_id3) {
					uni.navigateTo({
						url: '../shaddress/shaddress'
					})
				} else {
					uni.showToast({
						icon: "none",
						title: "请你微信授权"
					})
				}

			}
		}
	}
</script>

<style>
	.user {
		width: 100%;
		background-color: #F3F3F3;
	}

	.user_top {
		width: 100%;
		height: 400upx;
		background-color: #FF4444;
		display: flex;
		flex-direction: row;
	}

	.user_info_box {
		width: 122upx;
		height: 122upx;
		border-radius: 50%;
		margin-left: 32upx;
		margin-top: 102upx;
		position: relative;
	}

	.user_info_btn {
		width: 270upx;
		height: 122upx;
		border-radius: 50%;
		position: absolute;
		top: 102upx;
		left: 32upx;
		opacity: 0;
	}

	.user_info_box image {
		width: 122upx;
		height: 122upx;
		border-radius: 50%;
		margin: 0 auto;
	}

	.user_text {
		display: flex;
		flex-direction: column;
		margin-left: 20upx;
		margin-top: 102upx;
	}

	.user_text1 {
		font-size: 36upx;
		color: #FFFFFF;
	}

	.user_text2 {
		font-size: 28upx;
		color: #FFFFFF;
		margin-top: 18upx;
	}

	.user_text3 {
		font-size: 28upx;
		color: #FFFFFF;
		margin-top: 42upx;
	}

	.user_main {
		width: 718upx;
		height: 244upx;
		background-color: #FFFFFF;
		border-radius: 16upx 16upx 0 0;
		margin: -94upx 16upx 38upx 16upx;
	}

	.user_top_t {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin: 0 34upx 20upx 48upx;
		padding-top: 38upx;
	}

	.user_top_t_text1 {
		font-size: 32upx;
		font-family: Source Han Sans SC Medium;
		color: #333333;
		display: inline-block;
	}

	.user_top_t_text2 {
		font-size: 24upx;
		color: #999999;
		display: inline-block;
	}

	.user_top_t_r {
		/* width: 20%; */
		/* height: 50upx; */
	}

	.user_top_t_r>image {
		width: 10upx;
		height: 16upx;
		padding-left: 10upx;
		padding-bottom: 3upx;
	}

	.user_top_f {
		display: flex;
		flex-direction: row;
		margin-top: 10upx;
	}

	.user_item {
		width: 25%;
	}

	.user_item_icon {
		width: 56upx;
		height: 56upx;
		margin: 0 auto;
		position: relative;
	}

	.user_item_icon text {
		display: inline-block;
		width: 30upx;
		height: 30upx;
		text-align: center;
		position: absolute;
		top: -17upx;
		right: -13upx;
		color: #FFFFFF;
		font-size: 26upx;
		background-color: #FF4444;
		border-radius: 50%;
	}

	.user_item_text {
		font-size: 24upx;
		color: #666666;
		text-align: center;
	}

	.user_item_icon image {
		width: 56upx;
		height: 56upx;
	}

	.user_footer {
		width: 718upx;
		height: 384upx;
		background-color: #FFFFFF;
		border-radius: 16upx 16upx 0 0;
		margin: 0 auto;
	}

	.user_footer_t {
		font-size: 32upx;
		font-family: Source Han Sans CN Medium;
		color: #333333;
		margin: 0 0 0 34upx;
		padding-top: 40upx;
	}

	.user_footer_f {
		margin-top: 20upx;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
	}

	.user_item1 {
		width: 25%;
		margin-top: 30upx;
	}

	.user_item_icon1 {
		width: 56upx;
		height: 56upx;
		margin: 0 auto;
	}

	.user_item_text1 {
		font-size: 24upx;
		color: #999999;
		text-align: center;
		position: relative;
	}

	.user-btnkf {
		width: 160upx;
		height: 90upx;
		position: absolute;
		left: 0;
		bottom: 0;
		opacity: 0.1;
	}

	.user_item_icon1 image {
		width: 56upx;
		height: 56upx;
	}
</style>
