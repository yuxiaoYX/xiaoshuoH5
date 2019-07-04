<template>
	<view>
		qqqqqqq
	</view>
</template>

<script>
	export default {
		data() {
			return {
				search_data: {
					"bookName": "",
					"bookAuthor": "",
					"bookNewestChapterName": "",
					"bookUpdateTime": "",
					"bookUrl": "",
					"bookImage":""
				}
			}
		},
		onLoad(e) {
			console.log(e);
			uni.request({
				url: 'http://127.0.0.1:8000/api/search/',
				method: 'GET',
				data: {
					searchkey: e.val
				},
				success: res => {
					console.log(res);
				},
				fail: () => {},
				complete: () => {}
			});
		},
		methods: {

		}
	}
</script>

<style>

</style>
