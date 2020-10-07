<template>
	<view class="dongshi">
		<view class="dongshi-top">
			<image src="../../static/img/lunb1.png"></image>
		</view>
		<view class="dongshi-main">
			<view class="dongshi-item">
				<label>姓名</label>
				<input name="name" v-model="name" placeholder="请填写个人姓名" />
			</view>
			<text class="line"></text>
			<view class="dongshi-item">
				<label>电话</label>
				<input name="phone"  v-model="phone" type='number' placeholder="请填写个人电话" />
			</view>
			<text class="line"></text>
			<view class="dongshi-item1">
				<label>地址</label>
				<!-- <text></text> -->
				<input name="address" v-model="address" type="text" />
				<view class="dongshi-item1-r">
					<!-- <text>请选择</text> -->
					<view class="dongshi-item1-icon" @click="btn_address">
						<image src="../../static/img/icon-dl.png"></image>
					</view>
				</view>
			</view>
			<text class="line"></text>

		</view>
		<view class="dongshi-item2">
			<checkbox :checked="flag" @click="changeFlag"></checkbox>
			<text class="dongshi-item2-text1">我已阅读并了解</text>
			<text class="dongshi-item2-text2" @click="agreement_btn">【全国董事申请协议】</text>
		</view>
		<view class="dongshi-Submit">
			<view class="dongshi-Submit-come" @click="btn_dongshi">
				立即支付{{shareholder_money}}元成为全国董事
			</view>
		</view>
		<view class="dongshi-footer">
			<view class="dongshi-footer-top">全国董事特权</view>
			<view class="dongshi-footer-bottom" v-html="shareholder"></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				address:'',
				flag:true,
				name:'',
				phone:'',
				shareholder:'',
				shareholder_money:''
			}
		},
		onLoad() {
			uni.request({
				url:'https://www.jrtjrt.top/index/common/common_lst',
				data:{
					type:3
				},
				method:'POST',
				success: (res) => {
					// console.log(res.data.data.shareholder_privilege);
					this.shareholder=res.data.data.shareholder_privilege;
					this.shareholder_money=res.data.data.shareholder_money;
				}
			})
		},
		onShow() {
			this.user_id=uni.getStorageSync('user_id');
			
		},
		methods: {
			changeFlag(e){
				this.flag=!this.flag;
				// console.log(this.flag)
			},
			btn_address() {
				var _that = this
				uni.chooseLocation({
					success: function(res) {
						// console.log('位置名称：' + res.name);
						// console.log('详细地址：' + res.address);
						_that.address = res.address;
						// console.log(address)
						// console.log('纬度：' + res.latitude);
						// console.log('经度：' + res.longitude);
					}
				});
			},
			agreement_btn(){
				uni.navigateTo({
					url:'../agreement/agreement?type=3'
				});
				
			},
			btn_dongshi(){
				if (this.name.length < 1) {
					uni.showToast({
						icon: 'none',
						title: '姓名不能为空'
					});
					return;
				}
				if (this.phone.length < 11) {
					uni.showToast({
						icon: 'none',
						title: '电话最短为 11 个字符'
					});
					return;
				}
				if (this.address.length < 1) {
					uni.showToast({
						icon: 'none',
						title: '地址不能为空'
					});
					return;
				}
				if(this.flag){
					uni.request({
						url:'https://www.jrtjrt.top/index/promotion_level/promotion_level',
						data:{
							user_id:this.user_id,
							name:this.name,
							phone:this.phone,
							city:this.address,
							level:3
						},
						method:'POST',
						success: (res) => {
							// console.log(res)
								uni.requestPayment({
								    provider: 'wxpay',
								    timeStamp: res.data.data.timeStamp,
								    nonceStr: res.data.data.nonceStr,
								    package: res.data.data.package,
								    signType: res.data.data.signType,
								    paySign: res.data.data.paySign,
								    success: function (res) {
								        // console.log('success:' + JSON.stringify(res));
										uni.redirectTo({
											url:"../dongshibution/dongshibution"
										})
								    },
								    fail: function (err) {
								        // console.log('fail:' + JSON.stringify(err));
										uni.showToast({
											icon:"none",
											title:"未支付"
										})
								    }
								});
						}
					})
				}else{
					uni.showToast({
						icon: 'none',
						title: '请阅读规则'
					});
				}
			}
		}
	}
</script>

<style>
	/* checkbox样式修改 */
	checkbox .wx-checkbox-input {
		width: 26upx;
		height: 26upx;
		/* border-radius: 50%; */
	}

	checkbox .wx-checkbox-input.wx-checkbox-input-checked {
		border: 2upx solid #ff4500;
		background: #ff4500;
		color: #FFFFFF !important;
	}

	checkbox .wx-checkbox-input.wx-checkbox-input-checked::before {
		font-size: 40upx;
	}

	.dongshi {
		width: 100%;
	}

	.line {
		width: 95%;
		height: 2upx;
		display: inline-block;
		margin: 0 auto;
		/* margin: 30upx 0upx 5upx 0upx; */
		background-color: #f2f2f2;
		text-align: center;
	}

	.dongshi-main {
		padding-left: 30upx;
	}

	.dongshi-top {
		width: 750upx;
		height: 360upx;
	}

	.dongshi-top>image {
		width: 100%;
		height: 100%;
	}

	.dongshi-item {
		padding: 30upx 0 0 0;
		display: flex;
		flex-direction: row;
		background-color: #FFFFFF;
		/* margin-top: 2upx; */
	}

	.dongshi-item>label {
		font-size: 30upx;
		color: #333333;
	}

	.dongshi-item>input {
		width: 78%;
		padding-left: 28upx;
		font-size: 30upx;
		color: #616161;
	}

	.dongshi-item1 {
		padding: 30upx 30upx 0 0;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		background-color: #FFFFFF;
		/* margin-top: 2upx; */
	}

	.dongshi-item1>label {
		font-size: 30upx;
		color: #333333;
	}

	.dongshi-item1>input {
		width: 80%;
		/* padding-left: 28upx; */
		font-size: 30upx;
		color: #616161;
	}

	.dongshi-item1-icon {
		width: 22upx;
		height: 40upx;
	}

	.dongshi-item1-icon>image {
		width: 100%;
		height: 100%;
	}

	.dongshi-item2 {
		padding: 30upx 30upx 30upx 30upx;
		/* display: flex; */
		/* flex-direction: row; */
		/* justify-content: space-between; */
		background-color: #FFFFFF;
		/* margin-top: 2upx; */
	}

	.dongshi-item2>label {
		font-size: 30upx;
		color: #333333;
	}

	.dongshi-item2>input {
		width: 72%;
		/* padding-left: 28upx; */
		font-size: 30upx;
		color: #616161;
	}

	.dongshi-item2-text1 {
		font-size: 24upx;
		color: #333333;
	}

	.dongshi-item2-text2 {
		font-size: 24upx;
		color: #284769;
	}

	.dongshi-Submit {
		width: 100%;
		padding: 30upx;
		background-color: #F5F5F5;
	}

	.dongshi-Submit-come {
		width: 92%;
		height: 80upx;
		line-height: 80upx;
		border-radius: 10upx;
		background-color: #FF3434;
		font-size: 30upx;
		color: #FFFFFF;
		text-align: center;
	}

	.dongshi-footer {
		padding-left: 30upx;
	}

	.dongshi-footer-top {
		margin-top: 30upx;
		font-size: 30upx;
		color: #000000;
		position: relative;
		padding-left: 14upx;
	}

	.dongshi-footer-top::before {
		content: "";
		position: absolute;
		height: 30upx;
		width: 4upx;
		top: 6upx;
		left: 0;
		border-left: 4upx solid #FF3434;

	}

	.dongshi-footer-bottom {
		margin-top: 32upx;
		font-size: 26upx;
		color: #333333;
	}
</style>
