<template>
	<view class="tixianmingxi">
		<view class="tixianmingxi-head">
			<view class="tixianmingxi-headbtn">
				<view :class="['tixianmingxi-btn',isActive==1?'active':'']" @click="btn_nav2(1)">待审核</view>
			</view>
			<view class="tixianmingxi-headbtn">
				<view :class="['tixianmingxi-btn',isActive==2?'active':'']" @click="btn_nav2(2)">审核通过</view>
			</view>
			<view class="tixianmingxi-headbtn">
				<view :class="['tixianmingxi-btn',isActive==3?'active':'']" @click="btn_nav2(3)">不通过</view>
			</view>
		</view>
		<view class="tixianmingxi-centre" v-if="isActive==1">
			<view class="tixianmingxi-centre-main" v-for="(item,index) in mingxi_list" :key="index" v-if="isShow==1">
				<view class="tixianmingxi-centre-left">
					<text class="tixianmingxi-centre-text1">订单号：{{item.id}}</text>
					<text class="tixianmingxi-centre-text2">提现金额{{item.money}}</text>
				</view>
				<view class="tixianmingxi-centre-right">待审核</view>
			</view>
			<view class="tixianmingxi-lack" v-if="isShow==0">
				<view class="tixianmingxi-lack-img">
					<image src="../../static/img/gocart/gocart-lack.png"></image>
				</view>
				<text>暂无提现记录</text>
			</view>
		</view>
		<view class="tixianmingxi-centre" v-if="isActive==2">
			<view class="tixianmingxi-centre-main" v-for="(item,index) in mingxi_list" :key="index" v-if="isShow==1">
				<view class="tixianmingxi-centre-left">
					<text class="tixianmingxi-centre-text1">订单号：{{item.id}}</text>
					<text class="tixianmingxi-centre-text2">提现金额{{item.money}}</text>
				</view>
				<view class="tixianmingxi-centre-right">审核通过</view>
			</view>
			<view class="tixianmingxi-lack" v-if="isShow==0">
				<view class="tixianmingxi-lack-img">
					<image src="../../static/img/gocart/gocart-lack.png"></image>
				</view>
				<text>暂无审核记录</text>
			</view>
		</view>
		<view class="tixianmingxi-centre" v-if="isActive==3">
			<view class="tixianmingxi-centre-main" v-for="(item,index) in mingxi_list" :key="index" v-if="isShow==1">
				<view class="tixianmingxi-centre-left">
					<text class="tixianmingxi-centre-text1">订单号：{{item.id}}</text>
					<text class="tixianmingxi-centre-text2">提现金额{{item.money}}</text>
				</view>
				<view class="tixianmingxi-centre-right">不通过</view>
			</view>
			<view class="tixianmingxi-lack" v-if="isShow==0">
				<view class="tixianmingxi-lack-img">
					<image src="../../static/img/gocart/gocart-lack.png"></image>
				</view>
				<text>暂无审核记录</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isActive:1,
				mingxi_list:'',
				isShow:0,
				current:1,
				length:12
			}
		},
		onLoad() {

		},
		onShow() {
			this.user_id=uni.getStorageSync('user_id');
				uni.request({
					url: 'https://www.jrtjrt.top/index/withdrawal/withdrawal_lst ',
					data: {
						user_id: this.user_id,
						type: this.isActive
					},
					method: 'POST',
					success: (res) => {
						// console.log(res.data)
						this.mingxi_list = res.data.data.data;
						if(res.data.data.data==''){
							this.isShow=0;
							// console.log(1)
							// console.log(this.detriment)
						}else{
							this.isShow=1;
							// console.log(2)
							// console.log(this.detriment)
						}
						// console.log(this.mingxi_list)
					}
				})
		},
		onReachBottom(){
			// console.log(2)
			var _that = this;
			// console.log(this.current)
			uni.request({
				url: 'https://www.jrtjrt.top/index/withdrawal/withdrawal_lst',
				data: {
					user_id:this.user_id,
					current:this.current,
					type: this.isActive,
					length:this.length
				},
				method:'POST',
				success: (res) => {
					// console.log(res);
					var data = res.data.data.data;
					
					if(data.current==1){
						_that.mingxi_list = data;
						_that.current = _that.current + 1;
					}else{
						if(data.length==0){
							uni.showToast({
								icon:'none',
								title:'没有更多内容了'
							})
						}else{
							_that.mingxi_list = _that.mingxi_list.concat(data);
							_that.current = _that.current + 1;
						}
						
					}
					
					
					// console.log(_that.shangpinList)
					// this.text = 'request success';
				}
			});
		},
		methods: {
			btn_nav2(type) {
				this.current=1;
				this.isActive = type;
				// console.log(this.isActive)
				uni.request({
					url: 'https://www.jrtjrt.top/index/withdrawal/withdrawal_lst ',
					data: {
						user_id: this.user_id,
						type: this.isActive,
						current:this.current,
						length:this.length
					},
					method: 'POST',
					success: (res) => {
						// console.log(res.data)
						this.mingxi_list = res.data.data.data;
						if(res.data.data.data==''){
							this.isShow=0;
							// console.log(1)
							// console.log(this.detriment)
						}else{
							this.isShow=1;
							// console.log(2)
							// console.log(this.detriment)
						}
						// console.log(this.mingxi_list)
					}
				})
			}
		}
	}
</script>

<style>
	.tixianmingxi{
		width: 100%;
		background-color: #F1F1F1;
	}
	.tixianmingxi-head {
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

	.tixianmingxi-headbtn {
		display: inline-block;
		width: 33%;
		height: 100%;
		/* padding-left: 20upx; */
		font-size: 28upx;
		color: #000000;
		line-height: 60upx;
		position: relative;
		text-align: center;
	}
	.tixianmingxi-btn{
		position: relative;
	}
	.tixianmingxi-headbtn .active::after {
		content: "";
		position: absolute;
		width: 78upx;
		height: 0;
		left: 50%;
		margin-left: -39upx;
		bottom: 0;
		border-bottom: 4upx solid #F21C24;
	}
	.tixianmingxi-centre{
		margin-top: 62upx;
		
	}
	.tixianmingxi-centre-main{
		padding: 30upx;
		background-color: #FFFFFF;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin-bottom: 10upx;
	}
	.tixianmingxi-centre-left{
		display: flex;
		flex-direction: column;
	}
	.tixianmingxi-centre-right{
		font-size: 28upx;
		text-align: center;
		margin-top: 20upx;
		color: #FF4444;
	}
	.tixianmingxi-centre-text1{
		font-size: 28upx;
		color: #000000;
	}
	.tixianmingxi-centre-text2{
		font-size: 24upx;
		color: #000000;
		margin-top: 18upx;
	}
	/* 缺 */
	.tixianmingxi-lack{
		width: 300upx;
		height: 300upx;
		margin-top:220upx;
		margin-left: 280upx;
	}
	.tixianmingxi-lack-img{
		width: 200upx;
		height: 200upx;
	}
	.tixianmingxi-lack-img>image{
		width: 100%;
		height: 100%;
	}
	.tixianmingxi-lack>text{
		font-size: 26upx;
		color: #808080;
		text-align: center;
		padding-left: 18upx;
		padding-top: 18upx;
	}
</style>
