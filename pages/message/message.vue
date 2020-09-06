<template>
	<view class='container'>
		<view>
			<h1>欢迎光临小站</h1>
		</view>
		<view class="owner">
			<text style="font-size: 10px;">文件来源皆为学校官网或各行政单位站点</text>
			<text style="font-size: 10px;">资源仍在持续更新</text>
		</view>
		<view class="head" v-if="ishow">
			<view class="point">
				
			</view>
			<view class="point">
				
			</view>
			<view class="point">
				
			</view>
			<view class="point">
				
			</view>
			<view class="point">
				
			</view>
		</view>
		<view class="words"  v-if="!ishow">
			<view class="owner">
				<text style="font-size: 7px;">网站作者:</text>
				<h2>Ø</h2>
			</view>
			<view class="owner">
				<text style="font-size: 7px;">数据库提供者:</text>
				<h5>江心之人</h5>
			</view>

			<view class="owner">
				<text style="font-size: 7px;">本人及小站开发人员为在校计科学生</text>
			</view>
			<view class="owner">
				<text style="font-size: 7px;">如有疑问或涉嫌侵权请联系本人</text>QQ:1016831404
			</view>
							
		</view>
		<view style="font-size: 4px;display: flex;flex-direction: column;align-items: center;">
			<text>© CopyRight 2020 codezero.top Inc.All Rights Reserved.</text>
			<text>备案号 浙ICP备19038210号-2</text>	
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return{
				ishow:true
			}
		},
		onLoad() {
			console.log("success")
		},
		onPullDownRefresh() {
			let _this = this;
		
			_this.init();
		},
		mounted() {
			let _this = this;
			_this.showowner()
		},
		methods:{
			init(){
				uni.hideNavigationBarLoading();	
				setTimeout(function(){
					uni.stopPullDownRefresh();
					that.ishow=true;
				},1000);
				
			},
			showowner(){
				let that = this;
				setTimeout(function(){
					that.ishow=false;
				},3000);
			}
		}
		
	}
</script>

<style scoped='sass'>
	.owner{
		display: flex;
		flex-direction: row;
		align-items: center;
		width: 80vw;
		justify-content: space-around;
		height: auto;
		margin: 20px;
	}
	.words{
		display: flex;
		flex-direction: column;
		align-items: center;
		width: 40vw;
		justify-content: space-around;
	}
	*{
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		
	}
	.container{
		width: 100%;
		height: 100vh;
		background:#fff;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-around;
	}
	.head{
		display: flex;
		align-items: center;
		justify-content: center;
		height: 4rem;
	}
	.point{
		width: 1em;
		height: 1em;
		border-radius:50% ;
		background: #fff;
		margin: 0.5em;
		user-select: none;
		position: relative;
	}
	.point::before{
		position: absolute;
		content: '';
		width: 100%;
		height: 100%;
		background: inherit;
		border-radius: inherit;
		animation: wave 2.5s ease-out infinite;
	}
	@keyframes wave{
		50%,
		70%{
			transform: scale(1.3);
		},
		80%,
		100%{
			opacity: 0;
		}
		
	}
	.point:nth-child(1){
		background:#7ef9ff ;
	}
	.point:nth-child(2){
		background: #89cff0;
	}
	.point:nth-child(3){
		background: #4682b4;
	}
	.point:nth-child(4){
		background: #0f52ba;
	}
	.point:nth-child(5){
		background: #000080;
	}
	.point:nth-child(1):before{
		animation-delay:0s ;
	}
	.point:nth-child(2):before{
		animation-delay:0.2s ;
	}
	.point:nth-child(3):before{
		animation-delay:0.4s ;
	}
	.point:nth-child(4):before{
		animation-delay:0.6s ;
	}
	.point:nth-child(5):before{
		animation-delay:0.8s ;
	}
</style>
