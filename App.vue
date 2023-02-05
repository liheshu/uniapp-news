<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	view{
		box-sizing: border-box;
	},
	// 去除滚动条
	scroll-view ::-webkit-scrollbar {  
		    display: none !important;  
		    width: 0 !important;  
		    height: 0 !important;  
		    -webkit-appearance: none;  
		    background: transparent;  
		}
</style>
