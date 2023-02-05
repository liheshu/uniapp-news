<template>
	<view class="user">
		<view class="top">
			<image src="../../static/images/history.png" mode=""></image>
			<text>浏览历史</text>
		</view>
		<view class="content">
			<view class="row" v-for="item in listArr">
				<newsbox :item="item" @click.native="goodtatil(item)"></newsbox>
			</view>
		</view>	
		<view class="nohistory" v-if="!listArr">
			<image src="../../static/images/nohis.png" mode="widthFix"></image>
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
		onShow(){
			this.getData()
		},
		methods:{
			goodtatil(item){
				// console.log('dem')
				// 跳转详情
				uni.navigateTo({
					url:`/pages/detai/detai?cid=${item.classid}&id=${item.id}`
				})
			},
			//获取缓存浏览记录
			getData(){
				let hisArr=uni.getStorageSync("historyArr")||[]
				this.listArr=hisArr
				console.log(this.listArr);
			}
		}
	}
</script>

<style lang="scss">
.user{
	.top{
		padding: 50rpx 0;
		background: #f8f8f8;
		color: #666;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 150rpx;
			height:150rpx;
		};
		text{
				font-size: 38rpx;
				padding-top: 20rpx;
			}
		
	}
}
	.content {
		padding: 30rpx;
			.row{
				border-bottom: 1px dotted #efefef;
				padding: 20rpx;
			}
	}
	.nohistory{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 400rpx;
		}
	}
</style>
