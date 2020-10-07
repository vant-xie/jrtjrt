<template>
	<view class="gocart">
		<view class="golist" v-for="(item,index) in list">
			<!-- 单选 -->
			<view class="golist-l">
				<checkbox-group class="check" @change="changeitem(item)">
					<checkbox value="item" :checked="item.flag"></checkbox>
				</checkbox-group>
			</view>
			<view class="golist-t">
				<image :src="item.src"></image>
			</view>
			<view class="golist-r">
				<view class="golist-r1">
					<text>{{item.name}}</text>
					<image src="../../static/member/delete.png" @click="del(item,index)"></image>
				</view>
				<view class="golist-r2">
					<text>￥{{item.price}}</text>
					<view class="list-q">
						<label class="jian" @click="btn_minute(index)">-</label>
						<input class="count" type="text" v-model="item.num" />
						<label class="jia" @click="btn_add(index)">+</label>
					</view>
				</view>
			</view>

		</view>
		<!-- 底部 -->
		<view class="button">
			<!-- 全选按钮 -->
			<view class="quanxuan">
				<checkbox-group class="allBtn" @change="allchange">
					<checkbox value="cg" :checked="allchecked" />全选
				</checkbox-group>
			</view>
			<!-- 总价 -->
			<view class="zongjia">
				<text>总价：<text style="color:#f83e3a">￥{{allprice}}</text></text>
			</view>
			<!-- 结算 -->
			<view class="jiesuan" @click="jiesuan">
				<text>结算</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		onShow () { // 不使用 onLoad 以及 mounted 是因为页面被缓存，数据更新不及时
		      try{
		        let userid = uni.getStorageSync('userid')    // 获取到用户的userid
		        let token = uni.getStorageSync('token')
		        if (userid && token) {   
		          request({
		            url: '/cart',
		            data: {
		              userid, token
		            }
		          }).then(res => {
		            if (res.data.code === '10019') {
		              toast({title:'请先登录'})
		              uni.navigateTo({
		                url: '/pages/login/login'
		              })
		            } else if (res.data.code === '10012') {
		              toast({title:'请先选购商品'})
		              this.flag = true  //这里的flag是用来判断单选的
		            } else {
		              toast({title:'购物车列表获取成功'})
		              this.flag = false
					        //赋值之前给 数据添加数据项  item.data
		              res.data.data.map( item => {
		                item.flag = true   // 给每一个数据添加一个flag，后续用来判断单选的状态
		              })
		              this.cartlist = res.data.data
		              console.log(this.cartlist)
		            }
		          })
		        } else {
		          toast({title:'请先登录'})
		          uni.navigateTo({
		            url: '/pages/login/login'    // 跳转至登录页面
		          })
		        }
		      }catch(e){
		
		      }       
		    },
		computed: {
			// 计算选中商品的总价
			totalPrice() {
				let totalPrice = 0;
				this.list.map(item => {
					item.flag ? totalPrice += item.num * item.price : totalPrice += 0
				})
				return totalPrice
			}

		},
		created: function() {
			console.log("第一次走", this.list)
			var price = 0;
			var list = this.list;
			for (var i = 0; i < list.length; i++) {

				if (list[i]) {
					console.log(list[i])
					price += list[i].num * list[i].price
				}
			}
			this.allprice = price;
			this.allchange()
		},
		//总价总数量方法   方便调用   再次多写了一次[可以跟初始化封装为一个方法]
		hh: function() {
			let totalPrice = 0;
			this.list.map(item => {
				item.flag ? totalPrice += item.num * item.price : totalPrice += 0
			})
			this.allprice = totalPrice
		},
		//减去
		btn_minute: function(index) {
			var list = this.list;
			var num = list[index].num;
			if (num > 1) {
				num = num - 1;
				list[index].num = num;
			} else {
				alert('再减就没有商品啦')
			}
			this.hh();
		},
		//添加
		btn_add: function(index) {
			var list = this.list;
			// 强转int类型
			var num = parseInt(list[index].num);
			if (num < 100) {
				num = num + 1;
				// console.log(num)
				list[index].num = num;
			} else {
				alert('商品仅限购买10件')
			}
			this.hh();


		},
		// 单选
		changeitem(item) {
			console.log(item, '刚进来的item')


			item.flag = !item.flag
			if (item.flag) {

				let test = this.list.every(item => {

					return item.flag === true



				})
				console.log(test, '让看看这是什么')
				this.hh()
				if (test) {
					this.allchecked = true

				} else {
					this.allchecked = false

				}

			} else {
				this.allchecked = false
				this.hh()

			}
		},
		allchange() {
			this.allchecked = !this.allchecked
			// 如果 allcheckked 为真  全选被选中时，那么设置每一项都被选中
			if (this.allchecked) {
				// console.log(this.allchecked)
				this.list.map(item => {
					// console.log(this.detriment)
					//第一种计算总数方法
					//全选时，计算总数
					// this.num++;

					//全选时，选中商品
					item.flag = true
					//全选时，计算总价


				})
			} else {
				this.list.map(item => {
					//第一种计算总数方法
					//取消全选时，总数为0 
					// this.num = 0;
					//取消全选时，总价为0
					this.allprice = 0;
					//取消全选时，不选中商品
					item.flag = false;
				})
			}
			//第二种计算总数方法
			//全选计算商品 反之取消全选时，总数为0 

			//商品全选时,选中商品，反之则不选中商品
			for (var i = 0; i < this.list.length; i++) {
				if (this.allchecked) {
					this.list.flag = true
					console.log('成功')
					this.hh()

				} else {
					this.list.flag = false
					console.log('失败')
					this.allprice = 0
				}
			}
		},
		//商品的删除
		del (item,index) {
		  let token = uni.getStorageSync('token')
		  request({
		    url: '/cart/delete',
		    data: {
		      token,
		      cartid: item.cartid
		    }
		  }).then(res => {
		    if (res.data.code === '10019') {
		      toast({title:'请先登录'})
		      uni.navigateTo({
		        url: '/pages/login/login'
		      })
		    } else {
		      toast({title:'删除数据成功'})
		      this.cartlist.splice(index, 1) // 删除当前的数据
		      // 如果点击删除 删完之后要显示没有数据了
		      this.cartlist.length === 0 ? this.flag = true : this.flag = false
		    }
		  })
		}
	}
</script>

<style>
	.gouwuche {}
</style>
