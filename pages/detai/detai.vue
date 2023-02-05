<template>
	<view class="detail">
		<view class="title">{{detail.title}}</view>
		<view class="info">
		<view class="author">作者:{{detail.author}}</view>
		<view class="time">发布时间：{{detail.posttime}}</view>
		</view>
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>
		</view>
		<view class="description">本站内容均采集与腾讯新闻，如果侵权请联系管理(1192709841@qq.com)进行整改删除，本站进行了内容采集不代表本站及作者观点，若有侵权请及时联系管理员。</view>
	</view>

</template>

<script>
	import {parseTime} from "@/utils/tool.js"
	export default {
		data() {
			return {
				Options:null,
				detail:{}
			};
		},
		onLoad(e){
			// console.log(e)
			this.Options=e;
			this.getDetail()
		},
		methods:{
			getDetail(){
				uni.request({
					url:'https://ku.qingnian8.com/dataApi/news/detail.php',
					data:this.Options,
					success:res=>{
						// console.log(res);
						res.data.content=res.data.content.replace(/<img/gi,'<img style="max-width:100%"')
						res.data.posttime=parseTime(res.data.posttime)
						this.detail=res.data
						this.savaHistory()
						uni.setNavigationBarTitle({
							title:this.detail.title
						})
					}
				})
			},
			savaHistory(){
				
				let historyArr=uni.getStorageSync("historyArr")||[] 
				let item={
					id:this.detail.id,
					classid:this.detail.classid,
					picurl:this.detail.picurl,
					title:this.detail.title,
					looktime:parseTime(Date.now())
				}
				let index = historyArr.findIndex(i=>{
					return i.id==this.detail.id
				})
				if(index>=0){
					historyArr.splice(index,1)
				}
				historyArr.unshift(item)
				historyArr=historyArr.splice(0,10)
				uni.setStorageSync("historyArr",historyArr)
			}
		}
	}
</script>

<style lang="scss">
	.detail{
		padding: 30rpx;
		.title{
			font-size: 46rpx;
			color: #333;
		}
		.info{
			background: #f6f6f6;
			padding: 20rpx;
			font-size: 22rpx;
			color: #666;
			display: flex;
			justify-content: space-between;
			margin: 40rpx 0;
		}
		.content{
			padding-bottom: 50rpx;
		}
		.description{
			background: #fef0f0;
			font-size: 26rpx;
			padding: 20rpx;
			color: #f89898;
			line-height: 1.8em;
		}
	}
</style>
