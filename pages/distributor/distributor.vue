<template>
	<view class="distributor">
		<view class="distributor-top">
			<image src="../../static/img/lunb1.png"></image>
		</view>
		<view class="distributor-main">
			<view class="distributor-item">
				<label>姓名</label>
				<input name="name" v-model="name" type='text' placeholder="请填写个人姓名" />
			</view>
			<text class="line"></text>
			<view class="distributor-item">
				<label>电话</label>
				<input name="phone"  v-model="phone" type='number' placeholder="请填写个人电话" />
			</view>
			<text class="line"></text>
			<view class="distributor-item1">
				<label>地址</label>
				<!-- <text></text> -->
				<input name="address" v-model="address"  type="text" />
				<view class="distributor-item1-r">
					<!-- <text>请选择</text> -->
					<view class="distributor-item1-icon" @click="btn_address">
						<image src="../../static/img/icon-dl.png"></image>
					</view>
				</view>
			</view>
			<text class="line"></text>

		</view>
		<view class="distributor-item2">
			<checkbox :checked="flag" @click="changeFlag"></checkbox>
			<text class="distributor-item2-text1">我已阅读并了解</text>
			<text class="distributor-item2-text2" @click="agreement_btn">【合伙人申请协议】</text>
		</view>
		<view class="distributor-Submit">
			<view class="distributor-Submit-come" @click="btn_distributor">
				立即支付{{distribution_money}}元成为合伙人
			</view>
		</view>
		<view class="distributor-footer">
			<view class="distributor-footer-top">合伙人特权</view>
			<view class="distributor-footer-bottom" v-html="distributor"></view>
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
				distributor:'',
				distribution_money:''
			}
		},
		
		onLoad() {
			uni.request({
				url:'https://www.jrtjrt.top/index/common/common_lst',
				data:{
					type:2
				},
				method:'POST',
				success: (res) => {
					// console.log(res.data.data.distribution_privilege);
					this.distributor=res.data.data.distribution_privilege;
					this.distribution_money=res.data.data.distribution_money;
				}
			})
		},
		onShow() {
			this.user_id=uni.getStorageSync('user_id')
		},
		methods: {
			btn_address() {
				var _that = this
				uni.chooseLocation({
					success: function(res) {
						// console.log('位置名称：' + res.name);
						// console.log('详细地址：' + res.address);
						_that.address = res.address;
						// // console.log(address)
						// console.log('纬度：' + res.latitude);
						// console.log('经度：' + res.longitude);
					}
				});
			},
			changeFlag(e){
				this.flag=!this.flag;
				// console.log(this.flag)
			},
			agreement_btn(){
				uni.navigateTo({
					url:'../agreement/agreement?type=2'
				})
			},
			btn_distributor(){	
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
							level:2
						},
						method:'POST',
						success: (res) => {
							console.log(res)
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
										url:"../distribution/distribution"
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

	.distributor {
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

	.distributor-main {
		padding-left: 30upx;
	}

	.distributor-top {
		width: 750upx;
		height: 360upx;
	}

	.distributor-top>image {
		width: 100%;
		height: 100%;
	}

	.distributor-item {
		padding: 30upx 0 0 0;
		display: flex;
		flex-direction: row;
		background-color: #FFFFFF;
		/* margin-top: 2upx; */
	}

	.distributor-item>label {
		font-size: 30upx;
		color: #333333;
	}

	.distributor-item>input {
		width: 78%;
		padding-left: 28upx;
		font-size: 30upx;
		color: #616161;
	}

	.distributor-item1 {
		padding: 30upx 30upx 0 0;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		background-color: #FFFFFF;
		/* margin-top: 2upx; */
	}

	.distributor-item1>label {
		font-size: 30upx;
		color: #333333;
	}

	.distributor-item1>input {
		width: 80%;
		/* padding-left: 28upx; */
		font-size: 30upx;
		color: #616161;
	}
	.distributor-item1-icon {
		width: 22upx;
		height: 40upx;
	}
	
	.distributor-item1-icon>image {
		width: 100%;
		height: 100%;
	}

	.distributor-item2 {
		padding: 30upx 30upx 30upx 30upx;
		/* display: flex; */
		/* flex-direction: row; */
		/* justify-content: space-between; */
		background-color: #FFFFFF;
		/* margin-top: 2upx; */
	}

	.distributor-item2>label {
		font-size: 30upx;
		color: #333333;
	}

	.distributor-item2>input {
		width: 72%;
		/* padding-left: 28upx; */
		font-size: 30upx;
		color: #616161;
	}
	.distributor-item2-text1{
		font-size: 24upx;
		color: #333333;
	}
	.distributor-item2-text2{
		font-size: 24upx;
		color: #284769;
	}

	.distributor-Submit {
		width: 100%;
		padding: 30upx;
		background-color: #F5F5F5;
	}

	.distributor-Submit-come {
		width: 92%;
		height: 80upx;
		line-height: 80upx;
		border-radius: 10upx;
		background-color: #FF3434;
		font-size: 30upx;
		color: #FFFFFF;
		text-align: center;
	}
	.distributor-footer{
		padding-left: 30upx;
	}
	.distributor-footer-top{
		margin-top: 30upx;
		font-size: 30upx;
		color: #000000;
		position: relative;
		padding-left: 14upx;
	}
	.distributor-footer-top::before{
		content: "";
		position: absolute;
		height: 30upx;
		width: 4upx;
		top: 6upx;
		left: 0;
		border-left: 4upx solid #FF3434;
		
	}
	.distributor-footer-bottom{
		margin-top: 32upx;
		font-size: 26upx;
		color: #333333;
	}
</style>
