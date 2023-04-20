<template>
	<view>
		<!-- 轮播图区域 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<!-- 循环渲染轮播图的 item 项 -->
			<swiper-item v-for="(item, i) in swiperList" :key="i">
				<view class="swiper-item">
					<!-- 动态绑定图片的 src 属性 -->
					<image :src="item.image_src"></image>
				</view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 轮播图数据列表
				swiperList: []
			};
		},
		onLoad() {
			// 调用获取轮播图的自定义方法
			this.getSwiperList()
		},
		methods: {
			// 获取轮播图数据的方法
			async getSwiperList() {
				// check
				console.log(uni.$http)
				console.log(this.swiperList)
				// 请求数据
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/swiperdata')
				// 打印查看数据
				console.log(res)
				// 返回失败
				if (res.meta.status !== 200) {
					return uni.showToast({
						title: "数据请求失败",
						duration: 1500, // 1.5秒后自动消失
						icon: 'none'
					})
				}
				// 挂载数据到data中的列表
				this.swiperList = res.message
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
</style>