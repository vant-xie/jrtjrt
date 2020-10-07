<template>
	<view class="aboutus">
		<view class="aboutus-item" v-html="adoutus"></view>
	</view>
</template>
<script>
	export default {
		onLoad(options) {
			var _that=this;
			uni.request({
					url: 'https://www.jrtjrt.top/index/about_us/about_us_lst_d',
					data: {
						
					},
					methods: "post",
					success: (res) => {
						
						var data = res.data.data;
						_that.adoutus = data.content;
						// console.log(_that.adoutus.content)
						// let con = _that.aboutus.content;
						// console.log(_that.adoutus)
						_that.adoutus = _that.adoutus.replace(/\<img /gi,"<img style='width: 100%;'")
						// console.log(_that.adoutus.replace(/\<img /gi,"<img style='width: 100%;'"));
					}
				})
		},
		data() {
			return {
				adoutus:''
			}
		},
		//分享
		onShareAppMessage() {
			
		},
		methods: {
			
		}
	}
</script>

<style lang="scss">
	.aboutus{
		width: 100%;
	}
.aboutus-item{
	// margin-left: 30upx;
	width: 100%;
}
</style>
