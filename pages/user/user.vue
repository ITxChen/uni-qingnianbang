<template>
	<view class="user">
		<view class="top">
			<image src="../../static/image/histroy.png" mode=""></image>
			<view class="text">浏览历史</view>
		</view>
		<view class="content">
			<view class="row" v-for="item in listArr">
					<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
			</view>
		</view>
		<view class="nohis" v-if="!listArr">
			<image src="../../static/image/sel.png" mode=""></image>
			<view class="text">
				暂无浏览记录
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr:[]
			};
		},
		onShow() {
			this.getData()
		},
		methods:{
			goDetail(item){
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			// 获取缓存的浏览记录
			getData(){
				let hisArr = uni.getStorageSync("historyArr") || []
				this.listArr = hisArr
				console.log(this.listArr)
			}
		}
	}
</script>

<style lang="scss">
.user{
	.top{
		padding: 50rpx 0;
		background-color: #f8f8f8;
		color: #555;
		display: flex; //图标居中
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 150rpx;
			height: 150rpx;
		}
		.text{
			padding-top: 20rpx;
			font-size: 38rpx;
		}
	}
	.content{
		padding: 30rpx;
		.row{
			border-bottom: 1px dashed #efefef;
			padding: 20rpx 0;
		}
	}
	.nohis{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 300rpx;
			height: 300rpx;
		}
		.text{
			font-size: 26rpx;
			color: #888;
		}
	}
}
</style>
