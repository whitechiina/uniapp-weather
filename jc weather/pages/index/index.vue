<template>
	<view class="container">
		<view class="top">
			<view class="subtitle">—— 更新于{{time}} ——</view>
			<!-- 用户信息模块 -->
			<view class="user">
				<!-- 头像 -->
				<open-data class="open-data-userAvatarUrl" type="userAvatarUrl"></open-data> 
				<!-- 昵称 -->
				<open-data class="open-data-userNickName line1" type="userNickName" lang="zh_CN"></open-data>
			</view>
			<view class="location">
				您的当前位置
			</view>
			<view class="city">
				<image class="weizhi" src="/static/images/位置.png"></image>{{city}}
				<view class="pm">
					{{primarypollutant}} {{quality}}
				</view>
			</view>
			<!-- 当前温度 -->
			<view class="winter">
				{{nowwinter}} <text class="du">℃</text>
			</view>
			<view class="weather">
				<text class="weather2">
					{{weather}}
				</text>
			</view>

			<view class="tip">
				温馨提示：当前天气{{Now}}, {{tip}}
			</view>
		</view>

		<!-- 滚动列表 -->
		<scroll-view id="list" scroll-y="true" :style="{height:navHeight+'px'}" show-scrollbar="false">
			<view v-for="(item,index) in listArray" :key="item.id">
				<view class="list_item" v-bind:class="[index % 2 == 0 ? 'list-item-alter' : '']">
					{{item.date}}
					{{item.week}}
					<view class="right">
						{{item.night.templow}}℃~{{item.day.temphigh}}℃
						{{item.day.weather}}
					</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				navHeight: 0, //滚动区域的所需高度
				city: 'loading...', //城市
				time: 'loading...', //正在加载中
				week: 'loading...', //星期
				nowwinter: '0', //温度
				listArray: [], //列表数据
				
				weather: 'loading...', //天气
				Now: '', //当前
				tip: '', //提示
				primarypollutant: '', //PM
				quality: '' //污染等级
			}
		},

		onLoad() {
			// 动态获取列表高度
			this.calcScrollHeight()

			var self = this;
			
			
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
							self.city = res.data.result.city
							self.time = res.data.result.updatetime
							self.winter = res.data.result.temphigh
							self.listArray = res.data.result.daily
							// 当前实况
							self.nowwinter = res.data.result.hourly[0].temp
							self.weather = res.data.result.hourly[0].weather
							self.Now = res.data.result.index[6].ivalue
							self.tip = res.data.result.index[6].detail
							self.primarypollutant = res.data.result.aqi.primarypollutant
							self.quality = res.data.result.aqi.quality
						}
					})
				}
			});
		},

		methods: {
			// 计算滚动区域高度
			calcScrollHeight() {
				let that = this;
				uni.getSystemInfo({ //调用uni-app接口获取屏幕高度
					success(res) { //成功回调函数
						that._data.pH = res.windowHeight //windoHeight为窗口高度，主要使用的是这个
						let titleH = uni.createSelectorQuery().select("#list"); //想要获取高度的元素名（class/id）
						titleH.boundingClientRect(data => {
							console.log(data)
							let pH = that._data.pH;
							that._data.navHeight = pH - data.top //计算高度：元素高度=窗口高度-元素距离顶部的距离（data.top）
						}).exec()
					}
				})
			},
		},
	}
</script>

<style>
	@import url("./index.css");
</style>
