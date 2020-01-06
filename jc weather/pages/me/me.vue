<template>
	<view class='about'>
		<view class='content'>
			<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
				<swiper-item>
					<view class="swiper-item">
						<image class="swiper-img" src="http://q26q9szem.bkt.clouddn.com/11.png" mode=""></image>
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">
						<image class="swiper-img" src="http://q26q9szem.bkt.clouddn.com/22.png" mode=""></image>
					</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item">
						<image class="swiper-img" src="http://q26q9szem.bkt.clouddn.com/33.png" mode=""></image>
					</view>
				</swiper-item>
			</swiper>

			<view class='item'>
				<view class='title'>联系开发者</view>
				<view class='i' @click="copy1">
					<view class='icon'>
						<image src='/static/images/github.png'></image>
					</view>
					<view class='text'>
						<view >GitHub (点击复制)</view>
						<view>{{github}}</view>  
					</view>
				</view>
				<view class='i' @click="copy2">
					<view class='icon'>
						<image src='/static/images/email.png'></image>
					</view>
					<view class='text'>
						<view>作者邮箱 (点击复制)</view>
						<view>{{email}}</view>
					</view>
				</view>
				<view class='i' @click="copy3">
					<view class='icon'>
						<image src='/static/images/qq.png'></image>
					</view>
					<view class='text'>
						<view>作者QQ (点击复制)</view>
						<view>{{qq}}</view>
					</view>
				</view>
				 
				 
				 <view class="btnn">
				 	<button class='btn1'open-type="contact">
				 	   <image class='btnImg' src='/static/images/wechat.png'></image>
				 	   <view>在线沟通</view>
				 	</button>
				 	<button class='btn2' open-type="share">
				 	   <image class='btnImg' src='/static/images/wechat.png'></image>
				 	   <view>分享出去</view>
				 	</button>
				 </view>
			</view>



		</view>
		<view class='my'>developed by jinyaowei.xyz</view>
	</view>
</template>

<script>
	import helper from '../../common/helper.js';  
	export default {
		data() {
			return {
				github: 'https://github.com/whitechiina',
				email: '1293616053@qq.com',
				qq: '1293616053',
				wx: '',
			};
		},
		methods: {
			copy1(e) {
				var self = this
				uni.setClipboardData({
					data: self.github,
					success(res) {
						uni.showToast({
							title: '复制成功',
							duration: 1000,
							icon: "checkmarkempty"
						})
					}
				})
			},
			copy2(e) {
				var self = this
				uni.setClipboardData({
					data: self.email,
					success(res) {
						uni.showToast({
							title: '复制成功',
							duration: 1000,
							icon: "checkmarkempty"
						})
					}
				})
			},
			copy3(e) {
				var self = this
				uni.setClipboardData({
					data: self.qq,
					success(res) {
						uni.showToast({
							title: '复制成功',
							duration: 1000,
							icon: "checkmarkempty"
						})
					}
				})
			},
			
			
			onShareAppMessage(res) {
			      return {
			        title: '还是很喜欢你,像天气预报说明天有雨',
			        path: '/pages/index/index',
					desc: '此类天气将持续到娶你为止^-^',
			      }
			    }
			
			
				
		}
	}
</script>

<style>
	@import url("./me.css");
</style>
