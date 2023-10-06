<template>
	<view class="home">
		<view class="scrollNav">
			<scroll-view scroll-x="true" class="navscroll">
				<view class="item " 
				:class="index == navIndex ? 'active':'' " 
				v-for="(item,index) in navArr" 
				:key = item.id
				@click="clickNav(index,item.id)"
				>
				{{item.classname}}
				</view>
			</scroll-view>
		</view>
		
		<view class="content">
			<view class="row" v-for="item in newsArr" :key='item.id'>
					<newsbox :item="item" @click.native="goDetail(item)"></newsbox>
			</view>
		</view>
		
		<view class="nodata" v-if="!newsArr.length">
			<image src="../../static/images/1-1 (5).jpg" mode="widthFix"></image>
		</view>
		
		<view class="loading">
			<view class=""></view>
			<view class="" v-if="loading ==1">数据加载中......</view>
			<view class="" v-if="loading ==2 " >没有更多了</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex:0,
				navArr:[],
				newsArr:[],
				currentPage:1,
				loading:0 // 0默认 /加载中 2 没有更多
				
			}
		},
		onLoad() {
			this.getNavData(),
			this.getNewsData()
		},
		//触低
		onReachBottom() {
			// console.log('到底了')
			if(this.loading == 2){
				return;
			}
			this.currentPage++;
			this.getNewsData()
			this.loading = 1
		},
		methods: {
			//点击导航切换高亮
			clickNav(e,id){
				this.navIndex = e
				this.currentPage=1
				this.loading == 0
				this.newsArr = []
				// console.log(id)
				this.getNewsData(id)
			},
			goDetail(item){
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			// 获取导航列表数据
			getNavData(){
				uni.request({
					url:'https://ku.qingnian8.com/dataApi/news/navlist.php',
					success:res =>{
						console.log(res)
						this.navArr = res.data
					}
				})
			},
			// 获取新闻列表
			getNewsData(id){
					uni.request({
						url:'https://ku.qingnian8.com/dataApi/news/newslist.php',
						data:{
							cid:id,
							page:this.currentPage
						},
						success:res =>{
							console.log(res)
							// if(res.data.length=0){
							//   this.loading = 2
							// }
							this.newsArr = [...this.newsArr,...res.data]
							if(res.data.length == 0){
								this.loading = 2
							}
						}
					})
			}
	}
	}
</script>

<style lang="scss">
	.navscroll{
		height: 100rpx;
		background-color: #F7F8FA;
		white-space: nowrap;
		position: fixed;
		top:var(--window-top);
		left: 0;
		z-index: 10;
		/deep/ ::-webkit-scrollbar { //穿透 去掉头部的滚动条
			width: 4px !important;
			height: 1px !important;
			overflow: auto !important;
			background: transparent !important;
			-webkit-appearance: auto !important;
			display: block;
		}
		.item{
			font-size: 40rpx;
			display: inline-block;
			line-height: 100rpx;
			padding: 0 30rpx;
			color: #333;
			&.active{
				color: #31C27C;
			}
		}
	}
	.content{
		padding: 30rpx;
		padding-top: 100rpx;
		.row{
			border-bottom: 1px dashed #efefef;
			padding: 20rpx 0;
		}
	}
	.nodata{
		display: flex;
		justify-content: center;
		image{
			width: 360rpx;
		}
	}
	.loading{
		text-align: center;
		font-size: 26rpx;
		color: #888;
		view{
			padding: 20rpx 0;
		}
		
	}
</style>
