<template>
	<view class="Myteam">
		<view class="Myteam-head">
			<view class="Myteam-headbtn">
				<view :class="['Myteam-btn',isActive==1?'active':'']" @click="btn_nav3(1)">一级</view>
			</view>
			<view class="Myteam-headbtn">
				<view :class="['Myteam-btn',isActive==2?'active':'']" @click="btn_nav3(2)">二级</view>
			</view>
		</view>
		<view class="Myteam-centre" v-if="isActive==1">
			<view class="Myteam-centre-main" v-for="(item,index) in team_list" :key="index" v-if="isShow==1">
				<view class="Myteam-centre-top">
					<view class="Myteam-centre-icon">
						<image :src="item.avatarUrl"></image>
					</view>
					<view class="Myteam-centre-item">
						<text class="Myteam-centre-text1">{{item.nickname}}</text>
						<text class="Myteam-centre-text2">注册时间：{{item.createtime}}</text>
					</view>
				</view>
				<view class="Myteam-centre-bottom">
					<text class="Myteam-centre-text3">消费：{{item.order_money}}元</text>
					<text class="Myteam-centre-text4">{{item.order_count}}个订单</text>
				</view>
			</view>
			<view class="tixianmingxi-lack" v-if="isShow==0">
				<view class="tixianmingxi-lack-img">
					<image src="../../static/img/gocart/gocart-lack.png"></image>
				</view>
				<text>暂无一级记录</text>
			</view>
		</view>
		<view class="Myteam-centre" v-if="isActive==2">
			<view class="Myteam-centre-main" v-for="(item,index) in team_list" :key="index" v-if="isShow==1">
				<view class="Myteam-centre-top">
					<view class="Myteam-centre-icon">
						<image :src="item.avatarUrl"></image>
					</view>
					<view class="Myteam-centre-item">
						<text class="Myteam-centre-text1">{{item.nickname}}</text>
						<text class="Myteam-centre-text2">注册时间：{{item.createtime}}</text>
					</view>
				</view>
				<view class="Myteam-centre-bottom">
					<text class="Myteam-centre-text3">消费：{{item.order_money}}元</text>
					<text class="Myteam-centre-text4">{{item.order_count}}个订单</text>
				</view>
			</view>
			<view class="tixianmingxi-lack" v-if="isShow==0">
				<view class="tixianmingxi-lack-img">
					<image src="../../static/img/gocart/gocart-lack.png"></image>
				</view>
				<text>暂无二级记录</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isActive:1,
				isShow:0,
				type:1,
				team_list:'',
				current:1,
				length:12
			}
		},
		onLoad() {

		},
		onShow() {
			this.user_id=uni.getStorageSync('user_id');
			var _that=this;
			uni.request({
				url:'https://www.jrtjrt.top/index/user/user_subordinate_lst',
				data:{
					user_id:this.user_id,
					type:1
				},
				method:'POST',
				success: (res) => {
					// console.log(res.data.data.data)
					_that.team_list=res.data.data.data;
					if(res.data.data.data==''){
						this.isShow=0;
						// console.log(1)
						// console.log(this.detriment)
					}else{
						this.isShow=1;
						// console.log(2)
						// console.log(this.detriment)
					}
					// console.log(_that.team_list)
				}
			})
		},
		onReachBottom(){
			// console.log(2)
			var _that = this;
			// console.log(this.current)
			uni.request({
				url: 'https://www.jrtjrt.top/index/user/user_subordinate_lst',
				data: {
					user_id:this.user_id,
					current:this.current,
					type:this.isActive,
					length:this.length
				},
				method:'POST',
				success: (res) => {
					// console.log(res);
					var data = res.data.data.data;
					// _that.team_list = data;
					if(data.current==1){
						_that.team_list = data;
						_that.current = _that.current + 1;
					}else{
						if(data.length==0){
							uni.showToast({
								icon:'none',
								title:'没有更多内容了'
							})
						}else{
							_that.team_list = _that.team_list.concat(data);
							_that.current = _that.current + 1;
						}
						
					}
					
					// console.log(_that.shangpinList)
					// this.text = 'request success';
				}
			});
		},
		methods: {
			btn_nav3(type){
				this.current = 1;
				this.isActive = type;
				var _that=this;
				uni.request({
					url:'https://www.jrtjrt.top/index/user/user_subordinate_lst',
					data:{
						user_id:this.user_id,
						type:type,
						current:this.current,
						length:this.length
					},
					method:'POST',
					success: (res) => {
						// console.log(res.data.data.data);
						_that.team_list=res.data.data.data;
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
			}
		}
	}
</script>

<style>
	.Myteam{
		width: 100%;
		background-color: #F1F1F1;
	}
	.Myteam-head {
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

	.Myteam-headbtn {
		display: inline-block;
		width: 49%;
		height: 100%;
		/* padding-left: 20upx; */
		font-size: 28upx;
		color: #000000;
		line-height: 60upx;
		position: relative;
		text-align: center;
	}
	.Myteam-btn{
		position: relative;
	}
	.Myteam-headbtn .active::after {
		content: "";
		position: absolute;
		width: 78upx;
		height: 0;
		left: 50%;
		margin-left: -39upx;
		bottom: 0;
		border-bottom: 4upx solid #F21C24;
	}
	.Myteam-centre{
		margin-top: 62upx;
	}
	.Myteam-centre-main{
		padding: 30upx;
		background-color: #FFFFFF;
		margin-bottom: 10upx;
	}
	.Myteam-centre-top{
		display: flex;
		flex-direction: row;
		border-bottom: 2upx solid #E6E6E6;
		padding-bottom: 30upx;
	}
	.Myteam-centre-bottom{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin-top: 30upx;
	}
	.Myteam-centre-icon{
		width: 80upx;
		height: 80upx;
	}
	.Myteam-centre-icon>image{
		width: 100%;
		height: 100%;
	}
	.Myteam-centre-item{
		display: flex;
		flex-direction: column;
		margin-left: 20upx;
	}
	.Myteam-centre-text1{
		font-size: 28upx;
		color: #000000;
	}
	.Myteam-centre-text2{
		font-size: 24upx;
		color: #000000;
		margin-top: 18upx;
	}
	.Myteam-centre-text3{
		font-size: 28upx;
		color: #000000;
	}
	.Myteam-centre-text4{
		font-size: 28upx;
		color: #000000;
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
