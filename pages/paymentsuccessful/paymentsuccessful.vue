<template>
	<view class="paymentsuccessful">
		<view class="paymentsuccessful-main">
			<view class="paymentsuccessful-main-item">
				<image src="../../static/img/successful.png"></image>
			</view>
			<text class="paymentsuccessful-text1">订单支付成功 <text class="paymentsuccessful-text2">¥{{allprice}}</text> </text>
		</view>
		<view class="paymentsuccessful-footer">
			<button class="paymentsuccessful-btn1" @click="btn1(1)">查看订单</button>
			<button class="paymentsuccessful-btn2" @click="btn2">返回首页</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				allprice:''
			}
		},
		onLoad(options) {
			this.allprice=options.allprice
		},
		methods: {
			btn1(type){
				uni.redirectTo({
					url:'../order/order?type='+type
				})
			},
			btn2(){
				uni.switchTab({
					url:'../main/main'
				})
			}
		}
	}
</script>

<style>
	.paymentsuccessful{
		width: 100%;
	}
	.paymentsuccessful-main{
		padding: 100upx 100upx 100upx 200upx;
		border-top: 2upx solid #F1F1F1;
	}
	.paymentsuccessful-main-item{
		width: 302upx;
		height: 302upx;
		padding-bottom: 36upx;
	}
	.paymentsuccessful-main-item>image{
		width: 100%;
		height: 100%;
	}
	.paymentsuccessful-text1{
		font-size: 32upx;
		color: #333333;
	}
	.paymentsuccessful-text2{
		font-size: 36upx;
		color: #333333;
	}
	.paymentsuccessful-footer{
		display: flex;
		flex-direction: row;
	}
	.paymentsuccessful-btn1{
		width: 300upx;
		height: 78upx;
		background-color: #FE5151;
		color: #FFFFFF;
		font-size: 28upx;
		line-height: 78upx;
		text-align: center;
	}
	.paymentsuccessful-btn2{
		width: 300upx;
		height: 78upx;
		font-size: 28upx;
		color: #999999;
		line-height: 78upx;
		text-align: center;
	}
</style>
