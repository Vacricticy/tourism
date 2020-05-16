<template>
	<view>
		{{content}}
	</view>
</template>

<script>
	export default {
		data() {
			return {
				content: ''
			}
		},
		onLoad(option) {
			this.content = option.content
		},
		methods: {

		}
	}
</script>

<style>

</style>
