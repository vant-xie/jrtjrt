<template>
	<view class="partner">
		<view class="partner-top">
			<image src="../../static/img/lunb1.png"></image>
		</view>
		<view class="partner-main">
			<view class="partner-item">
				<label>姓名</label>
				<input name="name" v-model="name" type='text' placeholder="请填写个人姓名" />
			</view>
			<text class="line"></text>
			<view class="partner-item">
				<label>电话</label>
				<input name="phone"  v-model="phone" type='number' placeholder="请填写个人电话" />
			</view>
			<text class="line"></text>
			<view class="partner-item1">
				<label>地址</label>
				<!-- <text></text> -->
				<input name="address" v-model="address"  type="text" />
				<view class="partner-item1-r">
					<!-- <text>请选择</text> -->
					<view class="partner-item1-icon" @click="btn_address">
						<image src="../../static/img/icon-dl.png"></image>
					</view>
				</view>
			</view>
			<text class="line"></text>

		</view>
		<view class="partner-item2">
			<checkbox :checked="flag" @click="changeFlag"></checkbox>
			<text class="partner-item2-text1">我已阅读并了解</text>
			<text class="partner-item2-text2" @click="agreement_btn">【vip申请协议】</text>
		</view>
		<view class="partner-Submit">
			<view class="partner-Submit-come" @click="btn_partner">
				立即支付{{partner_money}}元成为vip
			</view>
		</view>
		<view class="partner-footer">
			<view class="partner-footer-top">vip特权</view>
			<view class="partner-footer-bottom" v-html="partner"></view>
		</view>
		<!--弹出框  -->
		<view class="order-box" v-if="showModalStatus">
			<view class="order-box-item">
				<text class="order-box-text1">温馨提示</text>
			</view>
			<view class="order-box-item1">
				<text class="order-box-text2">您已经成为vip!</text>
				<!-- <text class="order-box-text4">物流单号：<text class="order-box-text5">1234567890112345</text></text> -->
				<!-- <text class="order-box-btn" @click="btn_copy">点击复制快递单号</text> -->
			</view>
			<button class="order-box-btn1" @click="btn_box1">确定</button>
		</view>
		<view v-if="showModalStatus" style="position: fixed;top: 0;left: 0;right: 0;bottom: 0;width: 100%;height: 100%;background: rgba(0,0,0,.5);z-index: 999;"
		 catchtouchmove="true"></view>
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
				showModalStatus:false,
				level:'',
				partner:'',
				partner_money:''
			}
		},
		
		onLoad() {
			
			uni.request({
				url:'https://www.jrtjrt.top/index/common/common_lst',
				data:{
					type:1
				},
				method:'POST',
				success: (res) => {
					// console.log(res.data.data.partner_privilege);
					this.partner=res.data.data.partner_privilege;
					this.partner_money=res.data.data.partner_money;
				}
			})
		},
		onShow() {
			this.user_id=uni.getStorageSync('user_id');
			// this.shxin();
			
		},
		methods: {
			//定位
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
					url:'../agreement/agreement?type=1'
				})
			},
			//刷新
			shxin(){
				var _that = this;
				uni.request({
					url:'https://www.jrtjrt.top/index/user/user_lst_d',
					data:{
						user_id:this.user_id
					},
					method:'POST',
					success: (res) => {
						// console.log(res.data.data.level);
						// _that.level=res.data.data.level;
						this.level=res.data.data.level;
						// console.log(this.level)
						if(this.level==0){
							this.showModalStatus=false;	
						}else if(this.level==1){
							this.showModalStatus=true;
						}else if(this.level==2){
							this.showModalStatus=true;
						}else if(this.level==3){
							this.showModalStatus=true;
						}
					}
				});
			},
			btn_partner(){
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
				// console.log(checked.value)
				// if(this.flag==false){
				// 	uni.showToast({
				// 		icon: 'none',
				// 		title: '未勾选合伙人申请协议'
				// 	});
				// 	return;
				// }
				if(this.flag){
					uni.request({
						url:'https://www.jrtjrt.top/index/promotion_level/promotion_level',
						data:{
							user_id:this.user_id,
							name:this.name,
							phone:this.phone,
							city:this.address,
							level:1
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
										uni.navigateTo({
											url:'../distribution/distribution'
										})
										// uni.navigateTo({
										// 	url:'../paymentsuccessful/paymentsuccessful?allprice='+allprice
										// })
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
			},
			btn_box1(){
				uni.navigateBack({
					
				})
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

	.partner {
		width: 100%;
		position: relative;
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

	.partner-main {
		padding-left: 30upx;
	}

	.partner-top {
		width: 750upx;
		height: 360upx;
	}

	.partner-top>image {
		width: 100%;
		height: 100%;
	}

	.partner-item {
		padding: 30upx 0 0 0;
		display: flex;
		flex-direction: row;
		background-color: #FFFFFF;
		/* margin-top: 2upx; */
	}

	.partner-item>label {
		font-size: 30upx;
		color: #333333;
	}

	.partner-item>input {
		width: 78%;
		padding-left: 28upx;
		font-size: 30upx;
		color: #616161;
	}

	.partner-item1 {
		padding: 30upx 30upx 0 0;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		background-color: #FFFFFF;
		/* margin-top: 2upx; */
	}

	.partner-item1>label {
		font-size: 30upx;
		color: #333333;
	}

	.partner-item1>input {
		width: 80%;
		/* padding-left: 28upx; */
		font-size: 30upx;
		color: #616161;
	}
	.partner-item1-icon {
		width: 22upx;
		height: 40upx;
	}
	
	.partner-item1-icon>image {
		width: 100%;
		height: 100%;
	}

	.partner-item2 {
		padding: 30upx 30upx 30upx 30upx;
		/* display: flex; */
		/* flex-direction: row; */
		/* justify-content: space-between; */
		background-color: #FFFFFF;
		/* margin-top: 2upx; */
	}

	.partner-item2>label {
		font-size: 30upx;
		color: #333333;
	}

	.partner-item2>input {
		width: 72%;
		/* padding-left: 28upx; */
		font-size: 30upx;
		color: #616161;
	}
	.partner-item2-text1{
		font-size: 24upx;
		color: #333333;
	}
	.partner-item2-text2{
		font-size: 24upx;
		color: #284769;
	}

	.partner-Submit {
		width: 100%;
		padding: 30upx;
		background-color: #F5F5F5;
	}

	.partner-Submit-come {
		width: 92%;
		height: 80upx;
		line-height: 80upx;
		border-radius: 10upx;
		background-color: #FF3434;
		font-size: 30upx;
		color: #FFFFFF;
		text-align: center;
	}
	.partner-footer{
		padding-left: 30upx;
	}
	.partner-footer-top{
		margin-top: 30upx;
		font-size: 30upx;
		color: #000000;
		position: relative;
		padding-left: 14upx;
	}
	.partner-footer-top::before{
		content: "";
		position: absolute;
		height: 30upx;
		width: 4upx;
		top: 6upx;
		left: 0;
		border-left: 4upx solid #FF3434;
		
	}
	.partner-footer-bottom{
		margin-top: 32upx;
		font-size: 26upx;
		color: #333333;
	}
	/* 弹出框 */
	.order-box{
		width: 602upx;
		position: fixed;
		/* bottom: 414upx; */
		left: 76upx;
		top: 200upx;
		z-index: 2000;
		background: #FFFFFF;
		/* padding: 30upx 72upx 30upx 72upx; */
		position: absolute;
		border-radius: 10upx;
	}

	.order-box-item{
		text-align: center;
		margin-top: 30upx;
	}
	.order-box-item1{
		display: flex;
		flex-direction: column;
		margin-top: 80upx;
		margin-bottom: 80upx;
		padding-left: 72upx;
	}
	.order-box-text1{
		font-size: 32upx;
		color: #666666;
	}

	.order-box-btn{
		font-size: 26upx;
		color: #FF3434;
		margin-top: 20upx;
	}
	.order-box-btn1{
		width: 460upx;
		height: 80upx;
		border-radius: 10upx;
		background-color: #FF3434;
		font-size: 30upx;
		color: #FFFFFF;
		text-align: center;
		margin-top: 52upx;
		margin-bottom: 52upx;
	}
</style>
