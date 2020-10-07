<template>
	<view class="commission">
		<view class="commission-main">
			<text class="commission-text1">用户名</text>
			<text class="commission-text2">二级分销</text>
			<text class="commission-text3">获得佣金：<text class="commission-text4">80</text>元</text>
		</view>
		<view class="commission-main">
			<text class="commission-text1">用户名</text>
			<text class="commission-text2">二级分销</text>
			<text class="commission-text3">获得佣金：<text class="commission-text4">80</text>元</text>
		</view>
		<view class="commission-main">
			<text class="commission-text1">用户名</text>
			<text class="commission-text2">二级分销</text>
			<text class="commission-text3">获得佣金：<text class="commission-text4">80</text>元</text>
		</view>
	</view>	
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		//分享
		onShareAppMessage() {
			
		},
		methods: {
			
		}
	}
</script>

<style>
	.commission{
		width: 100%;
		background-color: #F1F1F1;
	}
	.commission-main{
		margin-top:2upx;
		margin-bottom: 10upx;
		background-color: #FFFFFF;
		padding: 30upx;
		display: flex;
		justify-content: space-between;
	}
	.commission-text1{
		font-size: 26upx;
		color: #000000;
	}
	.commission-text2{
		font-size: 26upx;
		color: #000000;
	}
	.commission-text3{
		font-size: 26upx;
		color: #000000;
	}
	.commission-text4{
		font-size: 26upx;
		color: #FF5555;
	}
</style>
