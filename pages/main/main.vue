<template>
	<view class="main">
		<view class="lunb">
			<!-- 		<view v-if="hasLogin" class="hello">
			<view class="title">
				您好 {{userName}}，您已成功登录。
			</view>
			<view class="ul">
				<view>这是 uni-app 带登录模板的示例App首页。</view>
				<view>在 “我的” 中点击 “退出” 可以 “注销当前账户”</view>
			</view>
		</view> -->
			<!-- 		<view v-if="!hasLogin" class="hello">
			<view class="title">
				您好 游客。
			</view>
			<view class="ul">
				<view>这是 uni-app 带登录模板的示例App首页。</view>
				<view>在 “我的” 中点击 “登录” 可以 “登录您的账户”</view>
			</view>
		</view> -->
			<view class="page-section-spacing">
				<swiper class="swiper" indicator-dots="false" autoplay="true" interval="3000" duration="500">
					<!-- <swiper-item v-for="(item , index) in homeSlide" :key="index">
					<image :src="item.img" mode="aspectFill"></image>
				</swiper-item> -->
					<swiper-item v-for="(item,index) in lun_Img" :key="index" :data-id=item.id>
						<image :src="item.img"></image>
					</swiper-item>
				</swiper>
			</view>
		</view>
		<view class="gongneng">

			<view class="uni-flex">
				<view class="flex-item" @click="shangpinzhongxin">
					<view class="flex-item-iocn">
						<image src="../../static/img/shangpin1.png" mode="widthFix"></image>
					</view>
					<view class="flex-item-texe">商品中心</view>
				</view>
				<view class="flex-item" @click="partner">
					<view class="flex-item-iocn">
						<image src="../../static/img/hhr1.png" mode="widthFix"></image>
					</view>
					<view class="flex-item-texe">vip</view>
				</view>
				<view class="flex-item" @click="dongshi">
					<view class="flex-item-iocn">
						<image src="../../static/img/ds1.png" mode="widthFix"></image>
					</view>
					<view class="flex-item-texe">全国董事</view>
				</view>
				<view class="flex-item" @click="distribution">
					<view class="flex-item-iocn">
						<image src="../../static/img/fenx1.png" mode="widthFix"></image>
					</view>
					<view class="flex-item-texe">合伙人</view>
				</view>
				<view class="flex-item" @click="aboutus">
					<view class="flex-item-iocn">
						<image src="../../static/img/wuim1.png" mode="widthFix"></image>
					</view>
					<view class="flex-item-texe">关于我们</view>
				</view>
			</view>
		</view>
		<view class="shangpin">
			<view class="shangpin-top">
				<view class="shangpin-top-image">
					<image src="../../static/img/rmsp.png"></image>
				</view>

			</view>
			<view class="shangpin-content">
				<view class="shangpin-item" v-for="(item,index) in shangpinList" :key='item.id' :data-id="item.id" @click="details">
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
	import {
		mapState
	} from 'vuex'

	export default {
		computed: mapState(['forcedLogin', 'hasLogin', 'userName']),
		data() {
			return {
				lun_Img: '',
				shangpinList: '',
				level: '',
				current:1,
				length:12
			}
		},
		
		//分享
		onShareAppMessage() {
			return {
				path: "/pages/main/main?uid=" + this.user_id
			}
		},
		onLoad(options) {
			if(options.uid){
				uni.setStorageSync("fx_id",options.uid)
			}
			// console.log(this.user_id)
			var _that = this;
			uni.request({
					url: 'https://www.jrtjrt.top/index/rotation/rotation_lst',
					data: {
						text: 'uni.request'
					},
					success: (res) => {
						// console.log(res.data);
						var data = res.data.data;
						// console.log(data[0].img)
						// console.log(res)
						_that.lun_Img = data;
						// console.log(_that.lun_Img[0].img)
						this.text = 'request success';
					}
				}),
				uni.request({
					url: 'https://www.jrtjrt.top/index/goods/goods_lst',
					data: {
						text: 'uni.request'
					},
					success: (res) => {
						// console.log(res);
						var data = res.data.data.data;
						_that.shangpinList = data;
						// console.log(_that.shangpinList)
						this.text = 'request success';
					}
				});
			setTimeout(function() {
				// console.log('start pulldown');
				
			}, 1000);
			uni.startPullDownRefresh();

		},
		onPullDownRefresh() {
			// console.log('refresh');
			var _that = this;
			uni.request({
					url: 'https://www.jrtjrt.top/index/rotation/rotation_lst',
					data: {
						text: 'uni.request'
					},
					success: (res) => {
						// console.log(res.data);
						var data = res.data.data;
						// console.log(data[0].img)
						// console.log(res)
						_that.lun_Img = data;
						// console.log(_that.lun_Img[0].img)
						this.text = 'request success';
					}
				}),
			uni.request({
				url: 'https://www.jrtjrt.top/index/goods/goods_lst',
				data: {
					current:this.current
				},
				method:'POST',
				success: (res) => {
					// console.log(res);
					var data = res.data.data.data;
					_that.shangpinList = data;
					// console.log(_that.shangpinList)
					// this.text = 'request success';
				}
			});
			setTimeout(function() {
				uni.stopPullDownRefresh();
			}, 1000);
		},
		onReachBottom(){
			// console.log(2)
			var _that = this;
			// console.log(this.current)
			uni.request({
				url: 'https://www.jrtjrt.top/index/goods/goods_lst',
				data: {
					user_id:this.user_id,
					current:this.current,
					length:this.length
				},
				method:'POST',
				success: (res) => {
					// console.log(res);
					var data = res.data.data.data;
					// _that.shangpinList = data;
					if(data.current==1){
						_that.shangpinList = data;
						_that.current = _that.current + 1;
					}else{
						if(data.length==0){
							uni.showToast({
								icon:'none',
								title:'没有更多内容了'
							})
						}else{
							_that.shangpinList = data;
							// _that.shangpinList = _that.shangpinList.concat(data);
							_that.current = _that.current + 1;
						}
						
					}
					
					// console.log(_that.shangpinList)
					// this.text = 'request success';
				}
			});
		},
		onShow() {
			this.user_id = uni.getStorageSync('user_id');
			// console.log(this.user_id)
			uni.request({
				url: 'https://www.jrtjrt.top/index/user/user_lst_d',
				data: {
					user_id: this.user_id
				},
				method: 'POST',
				success: (res) => {
					// console.log(res.data.data.level);
					this.level = res.data.data.level;
					// console.log(this.level)
				}
			})
		},
		methods: {
			//轮播
			lunb(e) {
				let id = e.currentTarget.dataset.id || e.target.dataset.id;
				// console.log(id)
				uni.navigateTo({
					url: '/pages/details/details?id=' + id
				})
			},
			//商品中心
			shangpinzhongxin() {
				uni.navigateTo({
					url: '/pages/shangpizhongxin/shangpinzhongxin'
				})
			},
			//合伙人
			partner() {
				if (this.user_id) {
					if (this.level == 2) {
						uni.navigateTo({
							url: '../distribution/distribution'
						})
					} else if (this.level == 3) {
						uni.navigateTo({
							url: '../distribution/distribution'
						})
					} else if(this.level==1) {
						uni.navigateTo({
							url: '../distribution/distribution'
						})
					}else{
						uni.navigateTo({
							url:'../partner/partner'
						})
					}
					
				} else {
					uni.switchTab({
						url: '../user/user'
					})
					uni.showToast({
						icon: "none",
						title: "请你微信授权"
					})
				}

			},
			//董事
			dongshi() {
				if (this.user_id) {
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
					uni.switchTab({
						url: '../user/user'
					})
					uni.showToast({
						icon: "none",
						title: "请你微信授权"
					})
				}

			},

			//分销中心
			distribution() {
				if (this.user_id) {
					if (this.level == 2) {
						uni.navigateTo({
							url: '../distribution/distribution'
						})
					} else if (this.level == 3) {
						uni.navigateTo({
							url: '../distribution/distribution'
						})
					} else {
						uni.navigateTo({
							url:'../distributor/distributor'
						})
					}
				} else {
					uni.switchTab({
						url: '../user/user'
					})
					uni.showToast({
						icon: "none",
						title: "请你微信授权"
					})
				}


			},
			//关于我们
			aboutus() {
				uni.navigateTo({
					url: '/pages/aboutus/aboutus'
				})
				// uni.navigateTo({
				// 	url:'../QRcode/QRcode'
				// })
			},
			//商品详细
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
	.main {
		width: 100%;

	}

	.title {
		color: #8f8f94;
		margin-top: 25px;
	}

	.ul {
		font-size: 15px;
		color: #8f8f94;
		margin-top: 25px;
	}

	.ul>view {
		line-height: 25px;
	}

	.lunb {
		width: 100%;
		height: 360upx;
	}

	.swiper {
		height: 360upx;
	}

	swiper-item image {
		width: 100%;
		height: 360upx;
	}

	.gongneng {
		width: 100%;
		height: 200upx;
	}

	.uni-flex {
		width: 100%;
		height: 180upx;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
	}

	.flex-item {
		width: 20%;
		height: 100upx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.flex-item-iocn {
		width: 90upx;
		height: 90upx;
		/* background-color: #007AFF; */
		border-radius: 50%;
	}

	.flex-item-iocn image {
		width: 100%;
		height: 100%;
	}

	.flex-item-texe {

		height: 22upx;
		font-size: 22upx;

	}

	.shangpin {
		width: 100%;
		background-color: #F7F7F7;

	}

	.shangpin-top {
		width: 100%;

	}

	.shangpin-top-image {
		width: 696upx;
		height: 106upx;
		margin: 0 auto;
	}

	.shangpin-top image {
		width: 696upx;
		height: 106upx;
		display: inline-block;
		margin: 0 auto;

	}

	.shangpin-content {
		width: 100%;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
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
		/* height: 50upx; */
		/* 		display: flex;
		flex-direction: row ;
		justify-content: space-between; */
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
		/* 		width: 72upx;
		height: 26upx; */
		font-size: 32upx;
		/* margin-left: 10upx; */
	}
</style>
