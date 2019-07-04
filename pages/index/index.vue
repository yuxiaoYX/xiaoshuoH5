<template>
	<view>
		<view class="flex solid-bottom padding-xs align-center " v-for="(item,index) in shujiaList" :key="index">
			<view class="radius">
				<image :src="item.src" class="jfif" mode="aspectFit"></image>
			</view>
			<view class="radius">
				<view class="text-lg margin-tb-xs">
					<text class="cuIcon-read text-blue margin-right-xs"></text>{{item.name}}</view>
				<view class="text-gray text-df margin-tb-xs">
					<text class="cuIcon-infofill margin-right-xs"></text>{{item.chapter}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				"shujiaList":[
					{"name":"百炼成神","chapter":"1分钟前：第29章 最后一个承诺","src":"/static/book/百炼成神.jfif"},
					{"name":"逆天邪神","chapter":"11分钟前：第29章 逆天邪神","src":"/static/book/逆天邪神.jfif"},
					{"name":"神魂丹帝","chapter":"43分钟前：第245章 神魂丹帝","src":"/static/book/神魂丹帝.jfif"},
					{"name":"神级修炼系统","chapter":"55分钟前：第1229章 神级修炼系统","src":"/static/book/神级修炼系统.jfif"},
					{"name":"元尊","chapter":"1年前：第1章 元尊","src":"/static/book/元尊.jfif"},
				]
			}
		},
		onNavigationBarButtonTap(options) {
			console.log(options);
			if (options.index==0) {
				uni.navigateTo({
					url: '/pages/setting/setting',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			} else if(options.index==1){
				uni.navigateTo({
					url: '/pages/search/search',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.jfif {
		width: 160upx;
		height: 160upx;
	}
</style>
