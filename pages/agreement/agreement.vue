<template>
	<view class="agreement">
		<view class="agreement-item" v-html="agreement"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				agreement:''
			}
		},
		onLoad(options) {
			this.type=options.type;
			// console.log(this.type)
			this.user_id=uni.getStorageSync('user_id');
			uni.request({
				url:'https://www.jrtjrt.top/index/common/common_lst',
				data:{
					type:this.type
				},
				method:'POST',
				success: (res) => {
					// console.log(res.data.data);
					if(res.data.data.partner_agreement){
						this.agreement=res.data.data.partner_agreement
					}else if(res.data.data.distribution_agreement){
						this.agreement=res.data.data.distribution_agreement
					}else if(res.data.data.shareholder_agreement){
						this.agreement=res.data.data.shareholder_agreement
					}
					
				}
			})
		},
		methods: {
			
		}
	}
</script>

<style>
	.agreement-item{
		padding: 30upx;
	}
</style>
