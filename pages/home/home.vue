<template>
	<view>
		<!-- 轮播图区域 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<!-- 循环渲染轮播图的 item 项 -->
			<swiper-item v-for="(item, i) in swiperList" :key="i">
				<navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
					<!-- 动态绑定图片的 src 属性 -->
					<image :src="item.image_src"></image>
				</navigator>
			</swiper-item>
		</swiper>
		<!-- 分类导航区域 -->
		<view class="nav-list">
		   <view class="nav-item" v-for="(item, i) in navList" :key="i" @click="navClickHandler(item)">
		     <image :src="item.image_src" class="nav-img"></image>
		   </view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 轮播图数据列表
				swiperList: [],
				// 分类导航数据列表
				navList: [],
			};
		},
		onLoad() {
			// 调用获取轮播图的自定义方法
			this.getSwiperList()
			// 调用获取分类导航数据的自定义方法
			this.getNavList()
		},
		methods: {
			// 获取轮播图数据
			async getSwiperList() {
				// // check
				// console.log(uni.$http)
				// console.log(this.swiperList)
				// 请求数据
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/swiperdata')
				// 返回失败
				if (res.meta.status !== 200) {
					return uni.$showMsg()
				}
				// 挂载数据到data中的列表
				this.swiperList = res.message
				// 提示
				uni.$showMsg("数据请求成功！")
			},
			// 获取分类导航数据
			async getNavList() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/catitems')
				if (res.meta.status !== 200) return uni.$showMsg()
				console.log(res)
				this.navList = res.message
			},
			// nav-item 项被点击时候的事件处理函数
			navClickHandler(item) {
			  // 判断点击的是哪个 nav 
			  if (item.name === '分类') {
			    uni.switchTab({
			      url: '/pages/cate/cate'
			    })
			  }
			  // console.log(item)
			}
		}
	}
</script>

<style lang="scss">
	swiper {
		height: 330rpx;

		.swiper-item,
		image {
			width: 100%;
			height: 100%;
		}
	}
	.nav-list {
	  display: flex;
	  justify-content: space-around;
	  margin: 15px 0;
	
	  .nav-img {
	    width: 128rpx;
	    height: 140rpx;
	  }
	}
</style>