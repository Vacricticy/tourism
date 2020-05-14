<template>
	<view>
		<avatar selWidth="200px" selHeight="400upx" @upload="myUpload" :avatarSrc="url" avatarStyle="width: 200upx; height: 200upx; border-radius: 100%;">
		</avatar>
	</view>
</template>

<script>
	import avatar from "../../components/yq-avatar/yq-avatar.vue";
	export default {
		data() {
			return {
				url: "../../static/logo.png"
			}
		}, 
		methods: {
			myUpload(rsp) {
				this.url = rsp.path; //更新头像方式一
				//rsp.avatar.imgSrc = rsp.path; //更新头像方式二
			}
		},
		components: {
			avatar
		}
	}
</script>

<style>

</style>
