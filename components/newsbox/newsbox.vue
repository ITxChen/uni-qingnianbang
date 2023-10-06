<template>
	<view class="newsbox">
		<view class="pic">
			<image :src="item.picurl" mode="aspectFill"></image>
		</view>
		<view class="text">
				<view class="title">
					{{item.title}}
				</view>
				<view class="info" v-if="!item.looktime">
					<text>{{item.anthor}}</text>
					<text>{{item.hits}}浏览</text>
				</view>
				<view class="info" v-else>
					<text>浏览时间:{{item.looktime}}</text>
				</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"newsbox",
		props:{
			item:{
				type:Object,
				default(){
					return{
						title:'组件内默认的标题',
						anthor:'张三',
						hits:668,
						picurl:'../../static/images/1-1 (1).jpg',
						// looktime:"2023-3-2 16:53"
					}
				}
			}
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="scss">
.newsbox{
	display: flex;
	.pic{
		width: 230rpx;
		height: 160rpx;
		image{;
			width: 100%;
			height:100%;
		}
	}
	.title{
		font-size: 36rpx;
		color:#333;
		text-overflow: -o-ellipsis-lastline;
		overflow: hidden;				//溢出内容隐藏
		text-overflow: ellipsis;		//文本溢出部分用省略号表示
		display: -webkit-box;			//特别显示模式
		-webkit-line-clamp: 2;			//行数
		line-clamp: 2;					
		-webkit-box-orient: vertical;	//盒子中内容竖直排列			
	}
	.text{
		// border: 1px solid red;
		flex:1; //沾满剩下的部分
		
		display: flex; // 文字一上一下：
		flex-direction: column;  // 横向排列
		justify-content: space-between;  // 上下排列
		
		padding-left: 20rpx;
		.title{
			font-size: 38rpx;
		}
		.info{
			font-size: 26rpx;
			color: #999;
			text{
				padding-right: 30rpx;
			}
		}
	}
}
</style>