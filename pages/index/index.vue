<template>
	<view class="content">
		<home-header></home-header>
		<view class="swiper">
			<u-swiper :list="swiperList" keyName="videoCover" indicatorStyle="{right: 15px} "
				@change="e => current = e.current" :autoplay="false">
				<view slot="indicator" class="indicator">
					<view class="indicator__dot" v-for="(item, index) in swiperList" :key="index"
						:class="[index === current && 'indicator__dot--active']">
					</view>
				</view>
			</u-swiper>
		</view>
		<view class="videoType">
			<view :span="3" v-for="(item, index) in videoTypes">
				<view class="videoTypeBg">
					<view>
						<view :class="'videoTypeIcon '+item.icon" :style="item.style"></view>
					</view>
					<view class="videoTypeTitle">
						{{ item.title }}
					</view>
				</view>
			</view>
		</view>
		<video-list :type="item.type" :title="item.title" v-for="item in videoKinds"></video-list>
	</view>
</template>

<script>
	import homeHeader from '@/components/homeHeader/homeHeader.vue'
	import videoList from '@/components/videoList/videoList.vue'

	export default {
		components: {
			homeHeader,
			videoList
		},
		data() {
			return {
				videoKinds: [{
						type: 1,
						title: '最新电影'
					},
					{
						type: 2,
						title: '最新电视剧'
					},
					{
						type: 3,
						title: '最新综艺'
					},
					{
						type: 4,
						title: '最新动漫'
					}
				],
				swiperList: [],
				current: 0,
				videoTypes: [{
						"videoType": 1,
						"icon": "iconfont icon-dianying",
						"title": "电影",
						"style": "background-color: rgb(2, 171, 233)"
					},
					{
						"videoType": 2,
						"icon": "iconfont icon-dianshiju",
						"title": "电视剧",
						"style": "background-color: rgb(2, 171, 233)"
					},
					{
						"videoType": 3,
						"icon": "iconfont icon-zongyi",
						"title": "综艺",
						"style": "background-color: rgb(2, 171, 233)"
					},
					{
						"videoType": 4,
						"icon": "iconfont icon-dongman",
						"title": "动漫",
						"style": "background-color: rgb(2, 171, 233)"
					}
				]
			}
		},
		onLoad() {
			this.querySwiper()
		},
		methods: {
			querySwiper() {
				uni.request({
					url: this.path + '/video/get',
					method: 'GET',
					data: {
						pageNum: 1,
						pageSize: 20
					},
					success: (res) => {
						let result = res.data;
						let index = [];
						let count = 0;
						while (count <= 3) {
							let i = parseInt(Math.random() * 20);
							if (index.indexOf(i) == -1) {
								index.push(i);
								this.swiperList.push(result.data.rows[i]);
								count++;
							}
						}
					}
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	.indicator {
		@include flex(row);
		justify-content: center;

		&__dot {
			height: 6px;
			width: 6px;
			border-radius: 100px;
			background-color: rgba(255, 255, 255, 0.35);
			margin: 0 5px;
			transition: background-color 0.3s;

			&--active {
				background-color: #ffffff;
			}
		}
	}

	.swiper {
		padding: 55px 10px 0;
	}

	.videoType {
		margin-top: 10px;
		height: 150rpx;
		background-color: #f2f2f2f2;
		display: flex;
		margin-left: 3%;
		width: 94%;
		border-radius: 10px;
		justify-content: space-around;
		align-items: center;

	}

	.videoTypeTitle {
		font-size: 12px;
		text-align: center;
	}

	.videoTypeIcon {
		color: white;
		font-size: 20px;
		height: 32px;
		width: 38px;
		border-radius: 10px;

		text-align: center;
		padding-top: 7px;
	}
</style>