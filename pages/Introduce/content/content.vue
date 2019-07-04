<template>
	<view>
		<!-- 章节内容 -->
		<view class="book-content padding" @click="tbShows()">
			<view class="text-xxl">{{chapterLists[thisChapter][1]}}</view>
			<view class="chapt" v-for="(chap,index1) in chapters" :key="index1">{{chap}}</view>
		</view>
		<!-- 顶部遮罩 -->
		<view class="" v-if="tbShow">
			<view class="top-setting bg-black cu-bar animation-slide-top" style="{animationDelay:0.1s}">
				<view class="action">
					<text class="cuIcon-back" @click="backPage"></text>返回
				</view>
				<view><text>剑来</text></view>
				<view class="action" @click="showModal">
					<text class="cuIcon-sort"></text>
				</view>
			</view>
			<view class="bottom-setting bg-black animation-slide-bottom">
				<view class="flex justify-center">111</view>
				<view class="flex justify-center">222222</view>
				<view class="cu-bar">
					<text class="action" @click="switchChapter(thisChapter-1)">上一章</text>
					<text class="action" @click="showModal">目录</text>
					<text class="action" @click="switchChapter(thisChapter+1)">下一章</text>
				</view>
			</view>
		</view>
		<!-- 底部遮罩 -->
		<view class="cu-modal drawer-modal justify-start" :class="modalName" @tap="hideModal">
			<view class="cu-dialog basis-lg" @tap.stop="">
				<scroll-view scroll-y style="height: calc(100vh - 0upx);">
					<view class="cu-list menu text-left">
						<view class="padding-xs margin-lr-sm"><text class="text-xxl">小说名字</text><br /></view>
						<view class="padding-xs margin-lr-sm"><text class="text-xl text-gray">目录</text></view>
						<view class="cu-item arrow" v-for="(item,index2) in chapterLists" :key="index2" style="min-height: 70upx;">
							<view class="content" :class="thisChapter==index2 ? 'text-red':'text-gray'">
								<view @click="switchChapter(index2)">{{index2+1}}.{{item[1]}}</view>
							</view>
						</view>
					</view>
				</scroll-view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tbShow: false,
				modalName: null,
				CustomBar: 1,
				//章节内容
				chapters: '',
				//小说列表
				chapterLists: ['', ''],
				//小说列表网址
				chapterListUrl: '',
				//源名称
				source_name: '',
				//当前章节索引
				thisChapter: 0
			}
		},
		onLoad(e) {
			console.log(e);
			this.source_name = e['source']
			this.visitList(e)
		},
		methods: {
			tbShows() {
				this.tbShow = !this.tbShow;
			},
			//打开抽屉
			showModal(e) {
				console.log(e);
				this.modalName = "show";
			},
			//合上抽屉
			hideModal(e) {
				this.modalName = null
			},
			//返回上一个页面
			backPage() {
				uni.navigateBack({
					delta: 1
				});
			},
			//获取小说列表
			visitList(e) {
				uni.request({
					url: 'http://127.0.0.1:8000/api/booklist/',
					method: 'GET',
					data: e,
					success: res => {
						console.log(res);
						this.chapterLists = res.data['list_data']
						this.chapterListUrl = res.data['url']
						this.switchChapter(0)
						// this.chapter(this.chapterListUrl + this.chapterLists[0][0])
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//获取单个章节内容
			switchChapter(e) {
				this.thisChapter = e
				uni.request({
					url: 'http://127.0.0.1:8000/api/content/',
					method: 'POST',
					data: {
						source_name: this.source_name,
						chapterUrl: this.chapterListUrl + this.chapterLists[e][0],
					},
					success: res => {
						this.chapters = res.data[0]
					},
					fail: () => {},
					complete: () => {
						//切换章节后，回到顶部
						uni.pageScrollTo({
							scrollTop: 0,
							duration: 100
						});
					}
				});
				this.hideModal()
				
			}
		}

	}
</script>

<style>
	.book-content {
		width: 100%;
		height: 100%;
		position: absolute;
		z-index: 1;
	}

	.top-setting {
		width: 100%;
		min-height: 80upx;
		position: fixed;
		z-index: 2;
		opacity: 0.8;
		top: 0upx;
	}

	.bottom-setting {
		width: 100%;
		/* height: 300upx; */
		position: fixed;
		z-index: 2;
		opacity: 0.8;
		bottom: 0;
	}

	.chapt {
		line-height: 2em;
		text-indent: 2em;
	}
</style>
