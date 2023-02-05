<template>
	<view class="home">
			<scroll-view scroll-x="true" class="navscroll">
				<view class="item" :class="id==navIndex ? 'active':''" v-for="(item,id) in navArr" :key="item.id" @click="clickNav(id,item.id)">{{item.classname}}</view>
				
			</scroll-view>
		<view class="content">
			<view class="row" v-for="item in newsArr" :key="item.id">
				<newsbox :item="item"  @click.native="goodtatil(item)"></newsbox>
			</view>
		</view>
		<view class="nodata"  v-if="!newsArr.length">
			<image src="../../static/images/nopic.jpg" mode="widthFix"></image>
		</view>
		<view class="loading" v-if="newsArr.length">
			<view></view>
			<view v-if="loading==1">数据加载中...</view>
			<view  v-if="loading==2">没有更多数据~~~</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navArr:[],
				newsArr:[],
				currentPage:1,
				loading:0  //0默认 1加载中  2没有更多
			}
		},
		onReachBottom(){
			if(this.loading==2){
				return
			}
			this.currentPage++;
			this.loading=1
			// console.log('刷新');
			this.getNewsData()
		},
		
		onLoad(){
			this.getNavData();
			this.getNewsData();
		},
		methods: {
			// 点击导航切换
		clickNav(e,id){
			// console.log(e,id);
			this.navIndex=e
			this.currentPage=1;
			this.newsArr=[]
			this.loading=0
			this.getNewsData(id)
		},
		goodtatil(item){
			console.log(item)
			// 跳转详情
			uni.navigateTo({
				url:`/pages/detai/detai?cid=${item.classid}&id=${item.id}`
			})
		},
		// 获取列表数据
		getNavData(){
			uni.request({
				url:'https://ku.qingnian8.com/dataApi/news/navlist.php',
				success:res=>{
					// console.log(res);
					this.navArr=res.data
				}
			})
		},
		// 获取新闻列表
		getNewsData(id=50){
			uni.request({
				url:'https://ku.qingnian8.com/dataApi/news/newslist.php',
				data:{
					cid:id,
					page:this.currentPage
				},
				success:res=>{
					// console.log(res);
					if(res.data.length==0){
						this.loading=2
					}
					this.newsArr=[...this.newsArr,...res.data]
				}
			})
		}
		}
	}
</script>

<style lang="scss">
	.navscroll{
		height: 100rpx;
		background-color: #f7f8fa;	
		white-space: nowrap;
		position: fixed;
		left: 0;
		top: var(--window-top);
		z-index: 999;
		.item{
			font-size: 40rpx;
			display: inline-block;
			line-height: 100rpx;
			padding: 0 30rpx;
			color: #333;
			&.active{
				color: #31c27c;
			}
		}	
	}
	.content {
		padding: 30rpx;
		padding-top: 130rpx;
			.row{
				border-bottom: 1px dotted #efefef;
				padding: 20rpx;
			}
		}
		.nodata{
			display:flex;
			justify-content: center;
			image {
				width: 360rpx;
				height: 360rpx;
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
