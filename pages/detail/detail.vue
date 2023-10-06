<template>
	<view class="detail">
		<view class="title">{{detail.title}}</view>
		<view class="info">
			<view class="author">编辑：{{detail.author}}</view>
			<view class="time">发布日期：{{detail.posttime}}</view>
		</view>
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>			
		</view>
		<view class="description">
			内容仅仅为了学习，不存在侵权行为
	</view>
</view>
</template>

<script>
	import {parseTime} from "../../utils/tool.js"
	console.log(parseTime(1659101134))
	export default {
		data() {
			return {
				options:null,
				detail:{}
			};
		},
	//用于接受上个页面传递过来的参数
	onLoad(item) {
		this.options=item
		this.getDetail()
	},
	methods:{
		getDetail(){
			uni.request({
				url:"https://ku.qingnian8.com/dataApi/news/detail.php",
				data:this.options, //传参
				success:res=>{
					// console.log(res)
					res.data.posttime =  parseTime(new Date()) 
					res.data.content = res.data.content.replace(/<img/gi,'<img style = "max-width:100%"')
					this.detail = res.data
					
					this.savaHistory()
					
					uni.setNavigationBarTitle({
						title:this.detail.title
					})
				}
			})
		},
		savaHistory(){
			
			let historyArr= uni.getStorageSync("historyArr") || []
			let item = {
				id:this.detail.id,
				classid:this.detail.classid,
				picurl:this.detail.picurl,
				looktime:parseTime(new Date),
				title:this.detail.title
			}
			// 返回索引值
			let index = historyArr.findIndex(i=>{
				return i.id == this.detail.id
			})
			// 如果索引值大于等于0，则缓存中已经存在了
			if(index>=0){
				// 删掉存在的
				historyArr.splice(index,1)
			}
			
			historyArr.unshift(item)
			historyArr = historyArr.splice(0,10)
			uni.setStorageSync("historyArr",historyArr)
		}
		
	}
	
	}
</script>

<style lang="scss">
.detail{
	padding:30rpx;
	.title{
		font-size: 46rpx;
		color:#333;
	}
	.info{
		background: #F6F6F6;
		padding:20rpx;
		font-size: 25rpx;
		color:#666;
		display: flex;
		justify-content: space-between;
		margin:40rpx 0;
	}
	.content{
		padding-bottom:50rpx;	
			// /deep/ img{
			// 	max-width: 100%;
			// }
	}
	.description{
		background: #FEF0F0;
		font-size: 26rpx;
		padding:20rpx;
		color:#F89898;
		line-height: 1.8em;
	}
}
</style>
