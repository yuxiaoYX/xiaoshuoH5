<template>
	<view>
		<view class="cu-load load-modal" v-if="loadModal">
			<image src="/static/logo.png" mode="aspectFit"></image>
			<view class="gray-text">加载中...</view>
		</view>
		<view class="flex dashed-bottom padding-tb-lg">
			<view class="">
				<image :src="bookData['bookImg']" class="jpg" mode="aspectFit"></image>
			</view>
			<view class="">
				<view class="text-xxl padding-tb-xs"><text>{{bookData['bookName']}}</text></view>
				<view class="padding-tb-xs"><text>{{bookData['bookAuthor']}} | {{bookData['bookType']}}</text></view>
				<view class="padding-tb-xs"><text>{{bookData['bookUpdate']}}</text></view>
			</view>
		</view>
		<view class="padding-sm text-content dashed-bottom"><text>{{bookData['bookIntro']}}</text></view>
		<view class="flex flex-wrap align-center justify-center">
			<button class="cu-btn bg-red margin-lg padding-lg" @click="startReading(bookData['bookReaderUrl'])">开始阅读</button>
			<button class="cu-btn bg-red margin-lg padding-lg">加入书架</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				bookData: {
					bookAuthor: "",
					bookImg: "",
					bookIntro: "",
					bookName: ""
				},
				loadModal: false,
				sourceName: ''
			}
		},
		onLoad(e) {
			console.log(e);
			this.loadModal = true;
			this.sourceName = e.source
			uni.request({
				url: 'http://127.0.0.1:8000/api/introduce/',
				method: 'POST',
				data: {
					source_name: e.source,
					url: e.url
				},
				success: res => {
					this.bookData = res.data[0]
					console.log(this.bookData);
				},
				fail: () => {},
				complete: () => {
					this.loadModal = false;
				}
			});
		},
		methods: {
			startReading(e) {
				uni.navigateTo({
					url: '/pages/Introduce/content/content?source=' + this.sourceName + '&listUrl=' + e,
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.jpg {
		width: 240upx;
		height: 240upx;
	}
</style>
