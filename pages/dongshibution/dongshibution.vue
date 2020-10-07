<template>
	<view class="dongshibution">
		<view class="dongshibution-head">
			<view class="dongshibution-head-top">
				<view class="dongshibution-head-icon">
					<image :src="auser_list.avatarUrl"></image>
				</view>
				<view class="dongshibution-head-tiem">
					<text class="dongshibution-head-tiem-text1">{{auser_list.nickname}}</text>
					<text class="dongshibution-head-tiem-text2" v-if="auser_list.shareholder_time==0" @click="btnqf">已过期</text>
					<text class="dongshibution-head-tiem-text2" v-if="!auser_list.shareholder_time==0">有效期：{{auser_list.shareholder_time}}</text>
				</view>
			</view>
			<view class="disabled-head-bottom" v-if="!auser_list.shareholder_time==0">
				<view class="disabled-head-bottom-left">
					<text class="disabled-head-bottom-text1">可提现现金</text>
					<text class="disabled-head-bottom-text2">{{auser_list.money}}元</text>
				</view>
				<view class="disabled-head-bottom-right" @click="withdrawal">提现</view>
			</view>
		</view>
<!-- 		<view class="dongshibution-main">
			<view class="dongshibution-main-left">
				<text class="dongshibution-main-text1">可提现现金</text>
				<text class="dongshibution-main-text2">1000元</text>
			</view>
			<view class="dongshibution-main-right">
				<text class="dongshibution-main-text3">未结算佣金</text>
				<text class="dongshibution-main-text2">1000元</text>
			</view>
		</view> -->
		<view class="dongshibution_footer" v-if="!auser_list.shareholder_time==0">
			<view class="dongshibution_footer_f">
				<view class="dongshibution_item1" @click="btn_tixianmingxi">
					<view class="dongshibution_item_icon1">
						<image src="../../static/img/icon-tsmx.png"></image>
					</view>
					<view class="dongshibution_item_text1">提现明细</view>
				</view>
				<view class="dongshibution_item1" @click="btn_commiss">
					<view class="dongshibution_item_icon1">
						<image src="../../static/img/icon-yj.png"></image>
					</view>
					<view class="dongshibution_item_text1">董事佣金</view>
				</view>
<!-- 				<view class="dongshibution_item1" @click="btn_fenxiaodingdan">
					<view class="dongshibution_item_icon1" >
						<image src="../../static/img/icon-dd.png"></image>
					</view>
					<view class="dongshibution_item_text1">分销订单</view>
				</view> -->

<!-- 				<view class="dongshibution_item1" @click="btn_Myteam">
					<view class="dongshibution_item_icon1">
						<image src="../../static/img/icon-mys.png"></image>
					</view>
					<view class="dongshibution_item_text1">我的团队</view>
				</view>
				<view class="dongshibution_item1">
					<view class="dongshibution_item_icon1">
						<image src="../../static/img/icon-ewm.png"></image>
					</view>
					<view class="dongshibution_item_text1">推广二维码</view>
				</view> -->
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				auser_list:''
			}
		},
		onLoad() {
			this.user_id=uni.getStorageSync('user_id');
			uni.request({
				url:'https://www.jrtjrt.top/index/user/user_lst_d',
				data:{
					user_id:this.user_id
				},
				method:'POST',
				success: (res) => {
					// console.log(res.data.data)
					this.auser_list=res.data.data;
					// console.log(this.auser_list)
					// var level='',
					// if(this.auser_list.level==0){
					// 	this.level=
					// }
					
				}
			})
			// uni.request({
			// 	url:'https://www.jrtjrt.top/index/withdrawal/withdrawal_lst  ',
			// 	data:{
			// 		user_id:this.user_id
			// 	},
			// 	method:'POST',
			// 	success: (res) => {
			// 		console.log(res.data.data)
			// 		// this.auser_list=res.data.data;
			// 		// console.log(this.auser_list.level)
			// 		// var level='',
			// 		// if(this.auser_list.level==0){
			// 		// 	this.level=
			// 		// }
					
			// 	}
			// })
		},
		methods: {
			btnqf(){
				uni.navigateTo({
					url:'../dongshi/dongshi'
				})
			},
			withdrawal(){
				uni.navigateTo({
					url:"../withdrawal/withdrawal"
				})
			},
			btn_commiss(){
				uni.navigateTo({
					url:"../withdrawal/withdrawal"
				})
			},
			btn_fenxiaodingdan(){
				uni.navigateTo({
					url:"../fenxiaodingdan/fenxiaodingdan"
				})
			},
			btn_tixianmingxi(){
				uni.navigateTo({
					url:"../tixianmingxi/tixianmingxi"
				})
			},
			btn_Myteam(){
				uni.navigateTo({
					url:"../Myteam/Myteam"
				})
			}
		}
	}
</script>

<style>
	.dongshibution {
		width: 750upx;
		background-color: #F1F1F1;
	}

	/* head */
	.dongshibution-head {
		width: 750upx;
		height: 336upx;
		background-color: #FF4444;
		color: #FFFFFF;
	}

	.dongshibution-head-top {
		/* width: 100%; */
		padding-left: 32upx;
		padding-top: 42upx;
		padding-bottom: 42upx;
		display: flex;
		flex-direction: row;
	}

	.dongshibution-head-icon {
		width: 122upx;
		height: 122upx;
		border-radius: 50%;
	}

	.dongshibution-head-icon>image {
		width: 100%;
		height: 100%;
		border-radius: 50%;
	}

	.dongshibution-head-tiem {
		display: flex;
		flex-direction: column;
		width: 75%;
		padding-left: 20upx;
	}

	.dongshibution-head-tiem-text1 {
		font-size: 48upx;
		color: #FFFFFF;
		/* margin-top: 30upx; */
	}

	.dongshibution-head-tiem-text2 {
		font-size: 28upx;
		color: #FFFFFF;
		margin-top: 16upx;
	}

	.disabled-head-bottom {
		display: flex;
		justify-content: space-between;
		padding-left: 32upx;
		padding-top: 32upx;
		padding-right: 30upx;
		border-top: 2upx solid #FFFFFF;
		display: flex;
		flex-direction: row;
	}

	.disabled-head-bottom-left {
		display: flex;
		flex-direction: column;
	}

	.disabled-head-bottom-text1 {
		font-size: 28upx;
		color: #FFFFFF;
	}

	.disabled-head-bottom-text2 {
		font-size: 28upx;
		color: #FFFFFF;
		margin-top: 11upx;
	}

	.disabled-head-bottom-right {
		width: 100upx;
		height: 46upx;
		border: 2upx solid #FFFFFF;
		border-radius: 24upx;
		line-height: 46upx;
		text-align: center;
		color: #FFFFFF;
		font-size: 28upx;
		margin-top: 8upx;
	}

	/* main */
	.dongshibution-main {
		width: 100%;
		height: 68upx;
		background-color: #FFFFFF;
		display: flex;
		flex-direction: row;
		padding-top: 20upx;
		padding-bottom: 20upx;
	}

	.dongshibution-main-left {
		width: 50%;
		display: flex;
		flex-direction: column;
		position: relative;
		margin-top: ;
		text-align: center;
	}

	.dongshibution-main-left::after {
		content: "";
		position: absolute;
		width: 0;
		height: 40upx;
		top: 50%;
		right: 0;
		margin-top: -20upx;
		border-right: 2upx solid #DBDBDB;
	}

	.dongshibution-main-text1 {
		font-size: 26upx;
		color: #4CE24A;
	}

	.dongshibution-main-text2 {
		font-size: 28upx;
		color: #333333;
		margin-top: 10upx;
	}

	.dongshibution-main-right {
		width: 50%;
		display: flex;
		flex-direction: column;
		margin: 0 auto;
		text-align: center;
	}

	.dongshibution-main-text3 {
		font-size: 26upx;
		color: #F38C27;
	}

	/* footer */
	.dongshibution_footer {
		width: 750upx;
		/* height: 384upx; */
		/* background-color: #FFFFFF; */
		/* border-radius: 16upx 16upx 0 0; */
		/* margin-top: 20upx; */
		/* margin: 0 auto; */
	}

	.dongshibution_footer_f {
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
	}

	.dongshibution_item1 {
		width: 248upx;
		height: 168upx;
		background-color: #FFFFFF;
		padding-top: 79upx;
		margin-bottom: 2upx;
		border-right: 2upx solid #F1F1F1;
	}
	.dongshibution_item1:nth-child(3n+3){
		border-right: none;
	}

	.dongshibution_item_icon1 {
		width: 48upx;
		height: 48upx;
		margin: 0 auto;
	}

	.dongshibution_item_text1 {
		font-size: 28upx;
		color: #333333;
		text-align: center;
	}

	.dongshibution_item_icon1>image {
		width: 100%;
		height: 100%;
	}
	
</style>
