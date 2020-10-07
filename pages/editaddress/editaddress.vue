<template>
	<view class="editaddress">
		<view class="editaddress-main">
			<view class="editaddress-item">
				<label>收货人</label>
				<input name="name" type='text' @input="messagesearch" v-model="name" placeholder="请填写收货人" />
			</view>
			<view class="editaddress-item">
				<label>联系电话</label>
				<input name="phone" v-model="phone" type='number' placeholder="请填写联系电话" />
			</view>
			<view class="editaddress-item1">
				<label>所在地区</label>
				<!-- <text></text> -->
				<input name="address" v-model="address" type="text" />
				<view class="editaddress-item1-r">
					<!-- <text>请选择</text> -->

					<view class="editaddress-item1-icon" @click="btn_address">
						<image src="../../static/img/icon-dl.png"></image>
					</view>
				</view>

				<!-- <input type='text' placeholder="请填写收货人" /> -->
			</view>
			<!-- 			<view class="editaddress-item2">
				<label>详细地址</label>
				<input class="editaddress-item-input" type='text' placeholder="请填写详细地址" />
			</view> -->
		</view>
		<view class="editaddress-footer">
			<text @click="btn_edit">保存</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				address: '',
				name: '',
				phone: ''
			}
		},
		onLoad(options) {
			this.user_id = uni.getStorageSync('user_id');
			this.id=options.id;
			this.name=options.name;
			this.phone=options.phone;
			this.address=options.address;
			// console.log(this.id);
		},
		methods: {
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
			messagesearch(e) {
				// console.log(e.target.value)
			},
			btn_edit() {
				if(this.id){
					uni.request({
						url: 'https://www.jrtjrt.top/index/address/address_upd',
						data: {
							id:this.id,
							user_id: this.user_id,
							name: this.name,
							phone: this.phone,
							address: this.address
						},
						method: 'POST',
						success: (res) => {
							// console.log(res.data)
							if (res.data.code == 1) {
								uni.redirectTo({
									url: "../shaddress/shaddress"
								})
							} else if (res.data.code == 0)
					
								uni.showToast({
									icon: "none",
									title: res.data.message
								})
						}
					})
				}else{
					uni.request({
						url: 'https://www.jrtjrt.top/index/address/address_add',
						data: {
							user_id: this.user_id,
							
							name: this.name,
							phone: this.phone,
							address: this.address
						},
						method: 'POST',
						success: (res) => {
							// console.log(res.data)
							if (res.data.code == 1) {
								uni.redirectTo({
									url: "../shaddress/shaddress"
								})
							} else if (res.data.code == 0)					
								uni.showToast({
									icon: "none",
									title: res.data.message
								})
						}
					})
				}

			}
		}
	}
</script>

<style>
	.editaddress {
		width: 100%;
		background-color: #F1F1F1;
	}

	.editaddress-main {
		margin-bottom: 100upx;
	}

	.editaddress-item {
		padding: 30upx 0 30upx 30upx;
		display: flex;
		flex-direction: row;
		background-color: #FFFFFF;
		margin-top: 2upx;
	}

	.editaddress-item1 {
		padding: 30upx 30upx 30upx 30upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		background-color: #FFFFFF;
		margin-top: 2upx;
	}

	.editaddress-item1>label {
		font-size: 30upx;
		color: #333333;
	}

	.editaddress-item1>input {
		width: 72%;
		margin-top: -4upx;
		/* padding-left: 28upx; */
		font-size: 30upx;
		color: #616161;
	}

	.editaddress-item2 {
		padding: 30upx 30upx 30upx 30upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		background-color: #FFFFFF;
		margin-top: 2upx;
	}

	.editaddress-item2>label {
		font-size: 30upx;
		color: #333333;
	}

	.editaddress-item2>input {
		width: 72%;
		margin-top: -4upx;
		/* padding-left: 28upx; */
		font-size: 30upx;
		color: #616161;
	}

	.editaddress-item1-r>text {
		font-size: 30upx;
		color: #616161;
	}

	.editaddress-item1-r>input {
		width: 78%;
		margin-top: -4upx;
		padding-left: 28upx;
		font-size: 30upx;
		color: #616161;
	}

	.editaddress-item1-icon {
		width: 22upx;
		height: 40upx;
	}

	.editaddress-item1-icon>image {
		width: 100%;
		height: 100%;
	}

	.editaddress-item1-r {
		display: flex;
		flex-direction: row;
	}

	.editaddress-item>label {
		font-size: 30upx;
		color: #333333;
	}

	.editaddress-item>input {
		width: 78%;
		margin-top: -4upx;
		padding-left: 28upx;
		font-size: 30upx;
		color: #616161;
	}

	.editaddress-footer {
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

	.editaddress-footer>text {
		width: 100%;
		height: 100%;
		font-size: 30upx;
		text-align: center;
		line-height: 100upx;
		color: #FFFFFF;
	}
</style>
