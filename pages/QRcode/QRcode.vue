<template>
	<view class="QRcode" :style="{'background-image': 'url('+qrcode_img+')','background-size': '100%'}">
		<view class="QRcode-main">
			<view class="QRcode-img">
				<image :src="qrcode"></image>
			</view>
		</view>
		<view class="QRcode-btn">
			<button @click="save">保存分销二维码到手机</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				qrcode:'',
				qrcode_img:''
			}
		},
		onShow() {
			this.user_id=uni.getStorageSync('user_id')
			uni.request({
				url:'https://www.jrtjrt.top/index/user/user_qr_code',
				data:{
					user_id:this.user_id
				},
				method:'POST',
				success: (res) => {
					// console.log(res.data.data)
					this.qrcode=res.data.data;
				}
			});
			uni.request({
				url:'https://www.jrtjrt.top/index/common/common_img',
				data:{
					user_id:this.user_id
				},
				method:'POST',
				success: (res) => {
					console.log(res.data.data.img)
					this.qrcode_img=res.data.data.img;
				}
			})
		},
		methods: {
			save() {
				let _that=this;
				this.saveToAlbum()
				// uni.getSetting({
				// 	success: (res) => {
				// 		console.log(res)
				// 		if(res.authSetting["scope.writePhotosAlbum"]){
				// 			_that.saveToAlbum();
				// 		}
				// 	},
				// 	fail: (res) => {
						
				// 	}
				// })
			},
			saveToAlbum(err){
				// uni.chooseImage({
				//     count: 1,
				//     sourceType: ['camera'],
				//     success: function (res) {
				//         uni.saveImageToPhotosAlbum({
				//             filePath: res.tempFilePaths[0],
				//             success: function () {
				//                 console.log('save success');
				//             }
				//         });
				//     }
				// });
				uni.getImageInfo({
					src:this.qrcode,
					success(res) {
						// console.log(res)
						uni.saveImageToPhotosAlbum({
							filePath:res.path,
							success: (res) => {
								console.log(res)
							},
							fail: (res) => {
								console.log('保存失败', res)
							}
						})
					}
				})
				
			}
		}
	}
</script>

<style>
	.QRcode {
		width: 100%;
		background-color: #FC8B8B;
	}

	.QRcode-main {
		width: 100%;
		margin-top: 700upx;
		margin-bottom: 20upx;
	}

	.QRcode-img {
		width: 344upx;
		height: 344upx;
		background-color: #FFFFFF;
		margin: 0 auto;
	}
	.QRcode-img>image{
		width: 100%;
		height: 100%;
	}
	.QRcode-btn {}

	.QRcode-btn>button {
		width: 460upx;
		height: 80upx;
		line-height: 80upx;
		margin: 0 auto;
		color: #FFFFFF;
		font-size: 30upx;
		text-align: center;
		background-color: #FF3434;
	}
</style>
