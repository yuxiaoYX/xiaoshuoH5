<template>
	<view>
		<!-- 顶部搜索栏 -->
		<view class="cu-bar search bg-white">
			<view class="action">
				<text class="cuIcon-back" @click="backPage">返回</text>
			</view>
			<view class="search-form round">
				<text class="cuIcon-search"></text>
				<input @focus="InputFocus" @confirm="searchPage" :adjust-position="false" type="text" placeholder="搜索图片、文章、视频"
				 confirm-type="search" :value="inputValue"></input>
			</view>
			<!-- <view class="action">
				<button class="cu-btn bg-green shadow-blur round" @click="searchPage">搜索</button>
			</view> -->
			<view class="action" @click="showModal">
				<text class="cuIcon-triangledownfill"></text>{{modalTitle}}
			</view>
		</view>
		<!-- 搜索热词 -->
		<view class="padding bg-white" v-if="inputValue==''">
			<view class=""><text class="text-xl">搜索热词</text></view>
			<view class="flex flex-wrap padding-tb-xs">
				<view class="cu-tag round margin-xs" v-for="(item,index) in 8" :key="index">
					<!-- <view class='cu-tag round'>椭圆{{item}}</view> -->
					<text class="margin-xs" v-on:click="searchPage(item)">椭圆{{item}}</text>
				</view>
			</view>
		</view>
		<view class="flex solid-bottom padding-xs align-center " v-for="(item,index) in searchData" :key="index" @click="jieshao(item.bookUrl)"
		 v-else>
			<view class="radius">
				<image src="/static/book/百炼成神.jfif" class="jfif" mode="aspectFit"></image>
			</view>
			<view class="radius">
				<view class="text-lg margin-xs">
					<text class="cuIcon-read text-blue margin-right-xs"></text>{{item.bookName}}</view>
				<view class="text-gray text-df margin-xs">
					<view><text class="cuIcon-infofill"></text>作者:{{item.bookAuthor}}</view>
					<view><text class="cuIcon-infofill"></text>最新章节:{{item.bookNewestChapterName}}</view>
					<view><text class="cuIcon-infofill"></text>更新时间:{{item.bookUpdateTime}}</view>
				</view>
			</view>
		</view>

		<view class="cu-modal" :class="modalName">
			<view class="cu-dialog">
				<view class="margin-sm text-xl"><text class="text-black">选择在线书源</text></view>
				<view class="content padding-tb-sm solid" v-for="(item,index) in bookSource" :key="index" @click="clickModal(item)">
					<text class="text-lg text-grey">{{item}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inputValue: '',
				modalName: null,
				modalTitle: '',
				searchData: {
					bookAuthor: "",
					bookName: "",
					bookNewestChapterName: "",
					bookUpdateTime: "",
					bookUrl: ""
				},
				bookSource: [
					'爱奇文学', '追书神器'
				],

			}
		},
		methods: {
			// 获取焦点时，清空输入框中内容
			InputFocus(e) {
				// this.inputValue = '';
			},
			// 点击完成按钮时触发
			searchPage(e) {
				this.inputValue = e.detail.value;
				uni.request({
					url: 'http://127.0.0.1:8000/api/search/',
					method: 'GET',
					data: {
						source_name:this.modalTitle,
						searchkey: this.inputValue
					},
					success: res => {
						console.log(res);
						this.searchData = res.data;
					},
				});
				
			},
			backPage() {
				uni.navigateBack({
					delta: 1
				});
			},
			showModal(e) {
				this.modalName = "show";
			},
			hideModal(e) {
				this.modalName = null;
			},
			clickModal(e) {
				this.modalTitle = e;
				this.hideModal();
			},
			jieshao(e) {
				uni.navigateTo({
					url: '/pages/Introduce/Introduce?source=' + this.modalTitle + '&url=' + e,
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		onLoad() {
			this.modalTitle = this.bookSource[0];
		}
	}
</script>

<style>
	.jfif {
		width: 160upx;
		height: 160upx;
	}
</style>
