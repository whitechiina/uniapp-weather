<template>
	<view>
		<!-- 用户信息模块 -->
		<view class="userBox">
			<!-- 头像 -->
			<open-data class="open-data-userAvatarUrl" type="userAvatarUrl"></open-data> 
			<!-- 昵称 -->
			<open-data class="open-data-userNickName" type="userNickName" lang="zh_CN"></open-data>
		</view>

		<!-- 今日数据 -->
		<view class="data">
			<view class="title title1">
				➤ 空气质量
			</view>
			<view class='detail'>
				<view class='details'>
					<view>空气等级</view>
					<view>{{level}}</view>
				</view>
				<view class='details'>
					<view>空气指数</view>
					<view :style="{color: color}">{{aqi}}</view>
				</view>
				<view class='details'>
					<view>空气质量</view>
					<view>{{quality}}</view>
				</view>
				<view class='details'>
					<view>PM 2.5</view>
					<view>{{pm}}</view>
				</view>
			</view>
			<view class="tip">
				<text class="love">❤</text>温馨小贴士：{{affect}}{{measure}}
			</view>
		</view>


		<!-- 生活指数 -->
		<view class="data">
			<view class="title">
				➤ 生活指数
			</view>
			<view class="live" v-for="(item,index) in live" :key="index">
				<view class="icon">
					<image class="img" :src="`../../static/images/${index}.png`" :key="index"></image>
				</view>
				<view class="shujv">
					<text class="iname"><text class="one">{{item.iname}}</text>&nbsp;&nbsp;&nbsp;{{item.ivalue}}</text>
					<view class="det">{{item.detail}}</view>
				</view>
			</view>
		</view>

		<view class="my">
			developed by jinyaowei.xyz
		</view>
	</view>
</template>

<script>
	import helper from '../../common/helper.js';
	export default {
		data() {
			return {
				show: 'loading...',
				aqi: 'loading...', //空气质量
				color: 'loading...',
				quality: 'loading...', //空气质量
				pm: 'loading...', //pm
				level: 'loading...', //level
				affect: 'loading...', //影响
				measure: 'loading...',

				// 生活指数
				live: [],
				liveimg: ['../../static/images/lifestyle_ac.png'],

			}
		},
		onLoad: function(option) {
			wx.showShareMenu({
				withShareTicket: true
			})
			let self = this;

			

			// 获取用户位置经纬度
			uni.getLocation({
				type: 'wgs84',
				success: function(res) {
					const longitude = res.longitude;
					const latitude = res.latitude;
					// 请求数据接口获取经纬度,然后转换为城市
					uni.request({
						url: 'https://api.jisuapi.com/weather/query?appkey=4c623f62eaafecc1&location=' + latitude + ',' + longitude +
							'',
						header: {
							'content-type': 'application/json'
						},
						success: function(res) {
							console.log(res)
							self.color = res.data.result.aqi.aqiinfo.color
							let aqi = res.data.result.aqi
							self.aqi = aqi.aqi
							self.quality = aqi.quality
							self.pm = aqi.pm2_5
							self.level = aqi.aqiinfo.level
							self.measure = res.data.result.aqi.aqiinfo.measure
							self.affect = res.data.result.aqi.aqiinfo.affect

							self.live = res.data.result.index
						}
					})
				}
			});

		}
	}
</script>


<style>
	@import url("./search.css");
</style>
