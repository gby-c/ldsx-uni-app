<template>
	<view class="content">
		<view class="top">
			<view class="top-left">{{ title }}</view>
			<view class="top-right">更多</view>
		</view>
		<view class="main">
			<view v-for="v in videoList" class="main-bg">
				<view class="main-top">
					<image :src="v.videoCover" class="main-img"></image>
					<view class="score">
						<view class="sorce">{{v.doubanScore}}</view>
					</view>
				</view>
				<view class="video-name">
					{{v.videoTitle.length > 5 ? v.videoTitle.substring(0, 5)+'...' : v.videoTitle}}
				</view>
				<view class="main-bottom">
					<view class="video-type">
						{{ v.videoTypes.split(',').slice(0, 2).join(',') }}
					</view>
					<view class="iconfont"></view>
				</view>
			</view>
		</view>
		<view class="bottom iconfont icon-huanyihuan " @click="query">
			<text class="bottom-text">换一换</text>
			
		</view>
	</view>
</template>

<script>
	export default {
		//接受父组件传的值
		props: {
			type: 0,
			title: '',
		},
		data() {
			return {
				videoList: [],
				pageNum: 1
			}
		},
		methods: {
			queryVideoList() {
				uni.request({
					url: this.path + '/video/get',
					method: 'GET',
					data: {
						pageNum: this.pageNum,
						pageSize: 6,
						videoKind: this.type
					},
					success: (res) => {
						let totol = res.data.data.total;
						let pages = parseInt((totol + 5) / 6);
						if (this.pageNum === pages) {
							this.pageNum = 0;
						}
						this.videoList = res.data.data.rows;
					}
				})
			},
			query() {
				this.pageNum++;
				this.queryVideoList();
			}
		},
		mounted() {
			this.queryVideoList();
		}
	}
</script>

<style scoped lang="scss">
	@import url(@/components/videoList/videoList.css);
</style>