<template>
	<view class="fenxiaodingdan">
		<view class="fenxiaodingdan-main" v-for="(item,index) in dingdan_list" :key="index" v-if="isShow==1">
			<view class="fenxiaodingdan-main-top">
				<text class="fenxiaodingdan-text1">订单号：{{item.order_number}}</text>
				<text class="fenxiaodingdan-text2">已完成</text>
			</view>
			<view class="fenxiaodingdan-main-bottom">
				<text class="fenxiaodingdan-text3">{{item.nickname}}</text>
				<text class="fenxiaodingdan-text4"  v-if="user_id==item.uid1">一级分销</text>
				<text class="fenxiaodingdan-text4"  v-if="user_id==item.uid2">二级分销</text>
				<text class="fenxiaodingdan-text5" v-if="user_id==item.uid1">获得佣金：<text class="fenxiaodingdan-text6">{{item.uid1_money}}</text>元</text>
				<text class="fenxiaodingdan-text5" v-if="user_id==item.uid2">获得佣金：<text class="fenxiaodingdan-text6">{{item.uid2_money}}</text>元</text>
			</view>
		</view>
		<view class="tixianmingxi-lack" v-if="isShow==0">
			<view class="tixianmingxi-lack-img">
				<image src="../../static/img/gocart/gocart-lack.png"></image>
			</view>
			<text>暂无记录</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dingdan_list:'',
				isShow:0,
				user_id:'',
				current:1,
				length:12
			}
		},
		onLoad() {
		},
		onShow() {
			this.user_id=uni.getStorageSync('user_id');
				// console.log(this.user_id)
				var _that=this;
				uni.request({
					url:'https://www.jrtjrt.top/index/goods_order/subordinate_order',
					data:{
						user_id:this.user_id
					},
					method:'POST',
					success: (res) => {
						// console.log(res.data.data.data)
						_that.dingdan_list=res.data.data.data;
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
		onReachBottom(){
			// console.log(2)
			var _that = this;
			// console.log(this.current)
			uni.request({
				url: 'https://www.jrtjrt.top/index/goods_order/subordinate_order',
				data: {
					user_id:this.user_id,
					current:this.current,
					length:this.length
				},
				method:'POST',
				success: (res) => {
					// console.log(res);
					var data = res.data.data.data;
					// _that.dingdan_list = data;
					if(data.current==1){
						_that.dingdan_list = data;
						_that.current = _that.current + 1;
					}else{
						if(data.length==0){
							uni.showToast({
								icon:'none',
								title:'没有更多内容了'
							})
						}else{
							_that.dingdan_list = _that.dingdan_list.concat(data);
							_that.current = _that.current + 1;
						}
						
					}
					
					// console.log(_that.shangpinList)
					// this.text = 'request success';
				}
			});
		},
		methods: {
			
		}
	}
</script>

<style>
	.fenxiaodingdan{
		width: 100%;
		background-color: #F1F1F1;
	}
	.fenxiaodingdan-main{
		padding: 30upx;
		background-color: #FFFFFF;
		margin-top: 2upx;
		margin-bottom: 10upx
	}
	.fenxiaodingdan-main-top{
		padding-bottom: 30upx;
		border-bottom: 2upx solid #E6E6E6;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}
	.fenxiaodingdan-main-bottom{
		padding-top: 30upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}
	.fenxiaodingdan-text1{
		font-size: 28upx;
		color: #000000;
	}
	.fenxiaodingdan-text2{
		font-size: 28upx;
		color: #FF4444;
	}
	.fenxiaodingdan-text3{
			font-size: 26upx;
			color: #000000;
	}
	.fenxiaodingdan-text4{
		font-size: 26upx;
		color: #000000;
	}
	.fenxiaodingdan-text5{
		font-size: 26upx;
		color: #000000;
	}
	.fenxiaodingdan-text6{
		font-size: 26upx;
		color: #FF5555;
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
