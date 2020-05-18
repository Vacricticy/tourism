<template>
	<view>
		<web-view :src="url"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				param: ''
			}
		},
		onLoad(option) {
			this.param = option.param
		},
		computed: {
			url() {
				return 'https://baike.baidu.com/item/' + this.param
			}
		},
		methods: {

		}
	}
</script>

<style>

</style>
