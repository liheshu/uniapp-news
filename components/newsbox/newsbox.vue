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
			<text>{{item.author}}</text>
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
					return {
						title:'组建内默认标题',
						author:'张三',
						hits:666,
						picurl:'../../static/images/history.png',
						looktime:'2023-02-05  10:10:15'
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

<style lang="less">
.newsbox{
	display: flex;
	.pic{
		width: 230rpx;
		height: 160rpx;
		image{
			width: 100%;
			height: 100%;
		}
	}
	.text{
		// border: 1px solid red;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		flex:1;
		padding-left: 20rpx;
		.title{
			font-size: 36rpx;
			color: #333;
			text-overflow: -o-ellipsis-lastline;
				overflow: hidden;				//溢出内容隐藏
				text-overflow: ellipsis;		//文本溢出部分用省略号表示
				display: -webkit-box;			//特别显示模式
				-webkit-line-clamp: 2;			//行数
				line-clamp: 2;					
				-webkit-box-orient: vertical;	//盒子中内容竖直排列
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