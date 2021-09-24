<template>
	<view class="home">
		<swiper indicator-dots circular>
			<swiper-item v-for="item in swipers" :key="item.goods_id">
				<image :src="item.image_src"></image>
			</swiper-item>
		</swiper>
		<view class="nav">
			<view class="nav_item" v-for="(item,index) in navs" :key="index" @click="navItemClick(item.path)">
				<view class="iconfont">
					<text>{{item.title}}</text>
				</view>
			</view>
		</view>
		<view class="hot_goods">
			<view class="tit">推荐商品</view>
		    <view class="goods_list">
				<view class="goods_item" v-for="item in goods" :key="item.id">
					<image :src="item.goods_small_logo" ></image>
					<view class="price">
						<text>{{item.goods_price}}</text>
						<text>{{item.goods_price + '0'}}</text>
					</view>
					<view class="name">
						{{item.goods_name}}
					</view>
				</view>
		    </view>
		</view>
	</view>
</template>
 
<script>
	export default {
		data() {
			return {
			  swipers: [],
			  goods: [],
			  navs: [
				  {
					  title: '黑马超市',
					  path: '/pages/goods/goods'
				  },
				  {
				  	title: '联系我们',
				  	  path: '/pages/contact/contact'
				  },
				  {
				  	 title: '社区图片',
				  	 path: '/pages/pics/pics'
				  },
				  {
				  	title: '学习视频',
				     path: '/pages/videos/videos'
				  }
			  ]
			}
		},
		onLoad() {
		  this.getSwipers()
		  this.getHotGoods()
		},
		methods: {
		async getSwipers () {
		 const res = await this.$myRequest({
			  url: '/home/swiperdata'
		  })
		   this.swipers = res.data.message
		},
		// 获取热门商品列表数据
		async getHotGoods () {
			const res = await this.$myRequest({
				url: '/goods/search'
			})
			console.log(res)
			this.goods = res.data.message.goods
		},
		// 导航点击处理函数
		navItemClick (url) {
			uni.navigateTo({
				url
			})
				console.log(url)
		}
	   }
	}
</script>

<style lang="scss">
	.home{
		swiper{
			width: 750rpx;
			height: 380rpx;
			image{
				height: 100%;
				width: 100%;
			}
		}
		.nav {
			display: flex;
			.nav_item {
				width: 25%;
				text-align: center;
				view{
					width: 120rpx;
					height: 120rpx;
					background: #b50e03;
					border-radius: 60rpx;
					margin: 10px auto;
					line-height: 120rpx;
					font-size: 50rpx;
					color: #FFFFFF;
				}
				text{
					font-size: 30rpx;
				}
			}
		}
		.hot_goods {
			background: #eee;
			overflow: hidden;
			margin-top: 10px;
			.tit {
				height: 50px;
				line-height: 50px;
				color: #b50e03;
				text-align: center;
				letter-spacing: 20px;
				background: #fff;
				margin: 7rpx 0;
			}
			.goods_list {
				padding: 0 15rpx;
				display: flex;
				flex-wrap: wrap;
				justify-content: space-between;
				.goods_item {
					background: #fff;
					width: 355rpx;
					margin: 10rpx 0;
					padding: 15rpx;
					box-sizing: border-box;
					image {
						width: 80%;
						height: 150px;
						display: block;
						margin: 0 auto;
					}
					.price{
						color: #b50e03;
						font-size: 36rpx;
						margin: 20rpx 0 5rpx 0;
						text:nth-child(2){
							color: #ccc;
							font-size: 28rpx;
							margin-left: 17rpx;
							text-decoration: line-through;
						}
					}
					.name {
						font-size: 28rpx;
						line-height: 50rpx;
						padding-bottom: 15rpx;
						padding-top: 10rpx;
					}
				}
			}
		}
	}
</style>
