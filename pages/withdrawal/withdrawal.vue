<template>
	<view class="withdrawal">
		<view class="withdrawal-head">
			<text class="withdrawal-head-text1">账户余额将提现到微信</text>
			<view class="withdrawal-head-item">
				<label>￥</label>
				<input type="digit" name="money" v-model="money"  />
			</view>
			<view class="withdrawal-head-bottom">
				<text class="withdrawal-head-text2">可提现余额：￥{{auser_list.money}}</text>
				<view class="withdrawal-head-btn" @click="btn_dc(1)">提现记录</view>
			</view>
		</view>
		<view class="withdrawal-main">
			<text class="withdrawal-main-text1">添加收款二维码(支付宝或微信)</text>
			<view class="withdrawal-main-item">
				<view class="withdrawal-main-icon1" @click="btn_withdrawal">
					<image :src="with_img"></image>
				</view>
				<!-- 				<view class="withdrawal-main-icon2">
					<image src="../../static/img/icon-addtp.png"></image>
				</view> -->
				<!-- <button></button> -->
			</view>
			<text class="withdrawal-main-text2">请上传正确的收款二维码，转账不了或转帐给他人，平台概不负责。</text>
		</view>
		<view class="withdrawal-footer" @click="btn_drawal">确定提现</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				with_img: '../../static/img/icon-addtp.png',
				money: '',
				auser_list: '',
				withdrawal_img:'',
				test_money:""
			}
		},
		onLoad() {
			this.user_id = uni.getStorageSync('user_id');
			uni.request({
				url: 'https://www.jrtjrt.top/index/user/user_lst_d',
				data: {
					user_id: this.user_id
				},
				method: 'POST',
				success: (res) => {
					// console.log(res.data.data)
					this.auser_list = res.data.data;
					// console.log(this.auser_list.level)
					// var level='',
					// if(this.auser_list.level==0){
					// 	this.level=
					// }

				}
			})
		},
		methods: {
			changeInp(event){
				console.log(event)
				if (event.target.keyCode!=46 && event.target.keyCode!=45 && event.target.keyCode<48 || event.target.keyCode>57) {
					console.log(1212,this.test_money,this.money)
					event.detail.value = this.test_money
					this.money = this.test_money
					console.log(this.money)
				}else{
					this.test_money = this.money;
				}
			},
			btn_withdrawal() {
				var _that = this;
				uni.chooseImage({
					count: 1,
					sizeType: ['original', 'compressed'],
					sourceType: ['album'],
					success: (res) => {
						console.log(res)
						_that.with_img = res.tempFilePaths;
						uni.uploadFile({
							url:"https://www.jrtjrt.top/index/img/addimg",
							filePath:res.tempFilePaths[0],
							name:"img",
							success(res) {
								// console.log(JSON.parse(res.data))
								var data=JSON.parse(res.data);
								// console.log(data.img)
								_that.withdrawal_img=data.img;
							},
							fail(err) {
								console.log(err)
							}
						})
					}
				})
			},
			btn_dc(type){
				uni.navigateTo({
					url:'../tixianmingxi/tixianmingxi?type='+type
				})
			},
			btn_drawal() {
				uni.request({
					url: 'https://www.jrtjrt.top/index/withdrawal/withdrawal_add',
					data: {
						user_id: this.user_id,
						img: this.withdrawal_img,
						money: this.money
					},
					method: 'POST',
					success: (res) => {
						this.code = res.data.code;
						// console.log(this.code)
						if (this.code == 1) {
							uni.showToast({
								icon: "none",
								title: res.data.message
							});
							uni.redirectTo({
								url:'../tixianmingxi/tixianmingxi'
							})
						} else if (this.code == 0) {
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
	.withdrawal {
		width: 100%;
		background-color: #F1F1F1;
		position: relative;
	}

	/* head */
	.withdrawal-head {
		width: 610upx;
		background-color: #FFFFFF;
		border-radius: 10upx;
		margin: 0 auto;
		margin-top: 20upx;
		padding: 40upx 42upx 42upx 40upx;
	}

	.withdrawal-head-text1 {
		font-size: 28upx;
		color: #7C838D;
	}

	.withdrawal-head-text2 {
		font-size: 28upx;
		color: #7C838D;

	}

	.withdrawal-head-item {
		font-size: 72upx;
		color: #121922;
		border-bottom: 2upx solid #E4E4EE;
		padding-bottom: 38upx;
		padding-top: 38upx;
		display: flex;
		flex-direction: row;
	}
	.withdrawal-head-item>input{
		height: 96upx;
	}
	.withdrawal-head-bottom {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		padding-top: 60upx;
	}

	.withdrawal-head-btn {
		font-size: 28upx;
		color: #14325B;
	}

	/* main */
	.withdrawal-main {
		padding: 50upx 30upx 0 30upx;
	}

	.withdrawal-main-text1 {
		font-size: 28upx;
		color: #333333;
	}

	.withdrawal-main-text2 {
		font-size: 22upx;
		color: #FF4444;
	}

	.withdrawal-main-item {
		display: flex;
		flex-direction: row;
		margin-top: 26upx;
		margin-bottom: 26upx;
	}

	.withdrawal-main-icon1 {
		width: 100upx;
		height: 100upx;
	}

	.withdrawal-main-icon1>image {
		width: 100%;
		height: 100%;
	}

	.withdrawal-main-icon2 {
		width: 100upx;
		height: 100upx;
		margin-left: 20upx;
	}

	.withdrawal-main-icon2>image {
		width: 100%;
		height: 100%;
	}

	/* footer */
	.withdrawal-footer {
		width: 690upx;
		height: 96upx;
		line-height: 96upx;
		border-radius: 48upx;
		color: #FFFFFF;
		text-align: center;
		background-color: #FF4444;
		/* margin:0 auto; */
		position: absolute;
		left: 30upx;
		bottom: 60upx;
	}
</style>
