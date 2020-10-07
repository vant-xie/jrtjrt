<template>
	<view class="shaddress">
		<view class="shaddress-main">
			<view class="shaddress-item" v-for="(item,index) in address_lst" :key="index" >
				<view class="shaddress-item-top" @click="btn_address1(item)">
					<view class="shaddress-left">
						<image src="../../static/img/my/icon-addr.png"></image>
					</view>
					<view class="shaddress-right">
						<view class="shaddress-right-top">
							<text class="shaddress-right-top-text1">{{item.name}}</text>
							<text class="shaddress-right-top-text2">{{item.phone}}</text>
						</view>
						<view class="shaddress-right-bottom">
							<text class="shaddress-right-bottom-text">收货地址：{{item.address}}</text>
						</view>
					</view>
				</view>
				<view class="shaddress-item-footer">
					<view class="shaddress-item-footer-top" :data-id="item.id" :data-name="item.name" :data-phone="item.phone" :data-address="item.address" @click="btn_addressbj">
						<view class="shaddress-item-icon1">
							<image src="../../static/img/icon-bj.png"></image>
						</view>
						<text class="shaddress-item-footer-text1">编辑</text>
					</view>
					<view class="shaddress-item-footer-bottom" :data-id="item.id" @click="shaddress_del">
						<view class="shaddress-item-icon2">
							<image src="../../static/img/shanchu.png"></image>
						</view>
						<text class="shaddress-item-footer-text1">删除</text>
					</view>
				</view>
			</view>
		</view>
		<view class="shaddress-footer" @click="btn_editaddress">
			<text>新增收货地址</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				address_lst: []
			}
		},
		onLoad() {
			this.user_id = uni.getStorageSync('user_id');

		},
		onShow() {
			this.btn_sx();
		},
		//分享
		onShareAppMessage() {
			
		},
		methods: {
			btn_sx(){
				uni.request({
					url: 'https://www.jrtjrt.top/index/address/address_lst',
					data: {
						user_id: this.user_id
					},
					method: 'POST',
					success: (res) => {
				
						this.address_lst = res.data.data;
						// console.log(this.address_lst);
						// if(res.data.code==1){
						// 	this.btn()
						// }
					}
				})
			},
			btn_editaddress() {
				uni.navigateTo({
					url: '../editaddress/editaddress'
				})
			},
			btn_address1(e) {
				// console.log(e)
				uni.setStorageSync('user_address',e)
			},
			//编辑
			btn_addressbj(e){
				// console.log(this.address_lst);
				let id = e.currentTarget.dataset.id||e.target.dataset.id;
				let name = e.currentTarget.dataset.name||e.target.dataset.name;
				let phone = e.currentTarget.dataset.phone||e.target.dataset.phone;
				let address = e.currentTarget.dataset.address||e.target.dataset.address;
				uni.navigateTo({
					url:'../editaddress/editaddress?id='+ id + "&name=" + name + "&phone=" + phone + "&address=" + address
				})
			},
			//删除
			shaddress_del(e){
				let id = e.currentTarget.dataset.id||e.target.dataset.id;
				var _that=this
				// this.user_address=uni.getStorageSync('user_address')
				// console.log(this.user_address)
				uni.request({
					url:'https://www.jrtjrt.top/index/address/address_del',
					data:{
						user_id:this.user_id,
						id:id
					},
					method:'POST',
					success: (res) => {
						// console.log(res.data.code)
						if(res.data.code==1){
							// this.btn_sx();
							uni.showToast({
								icon: "none",
								title: res.data.message
							})
							uni.request({
								url: 'https://www.jrtjrt.top/index/address/address_lst',
								data: {
									user_id: this.user_id
								},
								method: 'POST',
								success: (res) => {
							
									this.address_lst = res.data.data;
									this.user_address=uni.getStorageSync('user_address')
									let flags = true;
									// console.log(this.address_lst)
									for(var i=0;i<this.address_lst.length;i++){
										// console.log(this.address_lst[i].id)
										// console.log(this.user_address.id)
										if(this.address_lst[i].id==this.user_address.id){
											flags = false;
										}
									}
									// console.log(false)
									if(flags){
										uni.removeStorageSync('user_address')
									}else{
										
									}
										
									// }
									// console.log(this.user_address)
									// if(res.data.code==1){
									// 	this.btn()
									// }
								}
							})
							
						}else if(res.data.code==0){
							uni.showToast({
								icon: "none",
								title: res.data.message
							})
						}
					}
				})
			}
		}
	}
</script>

<style>
	.shaddress {
		width: 100%;
		background-color: #F1F1F1;
	}

	.shaddress-main {
		margin-bottom: 100upx;
		/* padding-right: 30upx; */
	}

	.shaddress-item {
		padding: 30upx 30upx 30upx 30upx;
		display: flex;
		flex-direction: column;
		background-color: #FFFFFF;
		margin-top: 2upx;
		margin-bottom: 10upx;
	}

	.shaddress-left {
		width: 58upx;
		height: 58upx;
		margin-top: 20upx;
	}

	.shaddress-left>image {
		width: 100%;
		height: 100%;
	}

	.shaddress-right {
		width: 85%;
		margin-left: 20upx;
	}

	.shaddress-right-top {
		width: 100%;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.shaddress-right-top-text1 {
		font-size: 30upx;
		color: #333333;
	}

	.shaddress-right-top-text2 {
		padding-left: 20upx;
		font-size: 30upx;
		color: #616161;
	}

	.shaddress-right-bottom {
		margin-top: 9upx;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.shaddress-right-bottom-text {
		font-size: 30upx;
		color: #333333;
	}

	.shaddress-footer {
		width: 100%;
		height: 100upx;
		position: fixed;
		left: 0;
		bottom: 0;
		background-color: #FFFFFF;
		display: flex;
		flex-direction: row;
		z-index: 999;
		background-color: #FF3434;

	}

	.shaddress-footer>text {
		width: 100%;
		height: 100%;
		font-size: 30upx;
		text-align: center;
		line-height: 100upx;
		color: #FFFFFF;
	}
	
	.shaddress-item-top{
		display: flex;
		flex-direction: row;
		border-bottom: 2upx solid #EFEFEF;
		padding-bottom: 20upx;
		
	}
	.shaddress-item-footer{
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
		/* padding-right: 30upx; */
		margin-top: 20upx;
	}
	.shaddress-item-footer-top{
		display: flex;
		flex-direction: row;
	}
	.shaddress-item-footer-bottom{
		display: flex;
		flex-direction: row;
		margin-left: 60upx;
	}
	.shaddress-item-icon1{
		width: 28upx;
		height: 28upx;
	}
	.shaddress-item-icon1>image{
		width: 100%;
		height: 100%;
	}
	.shaddress-item-icon2{
		width: 28upx;
		height: 28upx;
	}
	.shaddress-item-icon2>image{
		width: 100%;
		height: 100%;
	}
	.shaddress-item-footer-text1{
		font-size: 30upx;
		color: #333333;
	}
	.shaddress-item-footer-text2{
		font-size: 30upx;
		color: #333333;
	}
</style>
