<template>
	<view class="container">
<!-- 		<view class="headwords">
			<view>请输入您需要的文件</view>
			<view style="font-size: 20px;">支持关键字匹配</view>
		</view>
		<input type="text" v-model="keyword" class="uni-input" value="在此输入你想搜索的文件">
		<button type="default" @click="getdata">wd</button> -->
		<image src="../../static/logo.png" class="photo">
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
		<view class="mesbox">
			<view v-for="item in mesbox" class='message' v-bind:class="item.type":animation="animationData">
				<view v-html="item.content"></view>
			</view>
				
			</image>
			<view class='message getmes' v-if="loading" :animation="animationData" >
				<view class='pointbox' v-if="!received"><view class="point"></view><view class="point"></view><view class="point"></view><view class="point"></view><view class="point"></view></view>
				<view v-for="each in rec" class="filelist">
					<a v-bind:href="origin_url+each['owner']+'&wbfileid='+each['id']"><view>{{each["name"]}}</view></a>
				</view>
			</view>
		
		</view>
		<view class="input_box">
				<input type="text" class='keywords' v-model="keyword">
				<button id="butt" class="iconfont get_button" @click="send" type="default" >&#xe6ca;</button>
		</view>
		<!-- <uni-popup ref="popup" type="dialog">请输入内容</uni-popup> -->
	</view>
</template>

<script>
	
	export default {
		data() {
			return {
				name: '',
				keyword: '',
				mesbox:[{type:"getmes",content:'您好，请输入您想查询的文件'},{type:"getmes",content:'支持关键字匹配哦'}],
				loading:false,
				rec:[],
				received:false,
				number:0,
				height:-120,
				first:true,
				ishow:true,
				origin_url:"http://www.zjyc.edu.cn/system/_content/download.jsp?urltype=news.DownloadAttachUrl&owner="
			}
		},
		onShow: function(){
			let _this = this;
		    // 初始化一个动画
		    var animation = uni.createAnimation({
		        duration: 1000,
		        timingFunction:"ease-out",
				delay:200,
				transformOrigin: "50% 0 50%",  
				
		    })
		    _this.animation = animation;
		},
		//首先 页面一进来 加载一次数据
		onLoad() {
			
			// _this.getdata();
		},
		//在拉 在请求一次数据  下拉刷新
		onPullDownRefresh() {
			let _this = this;

			_this.init();
		},
		mounted() {
			let that = this;
			setTimeout(function(){
				that.ishow=false;
			},1400);
		},
		methods: {
			send(){
				let that = this;
				if(that.keyword!=''&&that.first){
					that.rec=[];
					that.mesbox.push({type:'sendmes',content:that.keyword});
					that.loading=true;
					that.loadshow();
					that.getdata();
					that.first=false;
					
					}
				else if(that.keyword==''){
					that.mesbox.push({type:'getmes',content:"请输入内容"});
					that.height=that.height-55;
				}
				else{
					that.received = false
					that.mesbox.push({type:'sendmes',content:that.keyword});
					that.getdata();
					that.loading=true;
					// that.mesbox[0].content=that.keyword;
				}
			},
			// 定义动画内容
			loadshow() {
			    // 定义动画内容
			    // this.animation.rotate(45).scale(2, 2).step()
				let _this = this;
				setTimeout(function(){
					_this.loading=true
				},1000);
				
				_this.animation.translateY(_this.height).step()
			    // 导出动画数据传递给data层
			    _this.animationData = _this.animation.export()
			},
			test(){
				console.log()
			},
			init(){
				let _this = this;
				_this.keyword='';
				uni.hideNavigationBarLoading();	
				setTimeout(function(){
					_this.rec=[];
					_this.mesbox=[]
					_this.mesbox.push({type:'getmes',content:"您好，请输入您想查询的文件"});
					_this.mesbox.push({type:'getmes',content:"抓紧查"});
					uni.stopPullDownRefresh();
				},1000);
				// _this.mesbox=[{type:"getmes",content:'您好，请输入您想查询的文件'},{type:"getmes",content:'支持关键字匹配哦'}];

			},
			getfile(e){
				let idnum = e["id"];
				console.log(idnum);
				 uni.navigateTo({
						url: ''+idnum,
					});
			},
			getdata() {
				let that = this;
				//获取数据的时候出现舒心的样式
				uni.showNavigationBarLoading();
				// 请求数据
				uni.request({
					// method:"GET",
					url: 'http://gfp.ronghuai312.top/jyeye/api.php?code='+this.keyword,
					success: (res) => {
						console.log(res)
						// document.getElementById('butt').style.cursor = 'not-allowed'
						setTimeout(function(){
							// that.loading = false;
							
							if(res["data"]["error"]==1){
								that.mesbox.push({type:'getmes',content:"请输入中文"})
								// that.rec = res["data"]["msg"]
								that.loading=false
								that.received=true
							}
							else if(res["data"]["num"]==0){
								that.mesbox.push({type:'getmes',content:"未查询到相应文件"})
								// that.rec = res["data"]["msg"]
								that.loading=false
								that.received=true
							}
							else{
								that.rec = res["data"]["info"]	
							}
							that.received=true
						},2000);
						// console.log(that.rec)
						uni.hideNavigationBarLoading();
						//这时候 请求完成后 停止刷新 的样式 也就是uni.showNavigationBarLoading();
					}
				})

			}
		}
	}
</script>

<style>
	.photo{
		position: fixed;
		width: 500px;
		height: 500px;
		bottom: 10px;
	}
	.filelist{
		text-decoration:underline;
		color:  	#DAA520;
	}
	.head{
		display: flex;
		align-items: center;
		justify-content: center;
		height: 4rem;
	}
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
		width: 100%;
		height: auto;
		display: flex;
		justify-content: flex-start;
		align-items: center;
		flex-direction: column;
	}
		
	.mesbox{
		width: 100%;
		overflow:hidden;
		height: auto;
		display: flex;
		flex-direction: column;
	}
	.message{
		margin-bottom: 20px;
		display: flex;
		flex-direction: row;
		/* height: auto; */
		width: 100%;
	}
	.input_box{
		position: fixed;
		bottom: 20vh;
		/* width: 80vw; */
		width: 10rem;
		height: 2.4rem;
		border-radius: calc(20px);
		/* border: #6E6E6E solid 1px; */
		display: flex;
		flex-direction: row;
		background-color:#fff;
		border: #0077AA solid 1px;
		justify-content: space-around;
	}
	.keywords{
		width: 8rem;
		height: 2.4rem;
		display: flex;
		flex-direction: row;
		align-items: center;
		text-align: center;
		color: #007AFF;
		font-size: 14px;
	}
	.get_button{
		width: 2rem;
		height: 2.4rem;
		font-size: 25px;
		color: #fff;
		flex-direction: row;
		display: flex;
		justify-content: center;
		align-items: center;
		border: #0077AA solid 1px;
		border-radius: 0px 15px 15px 0px;
		background-color: #4682b4;
		margin-left: 5px;
		
	}
	.getmes{
		width: 60%;
		background-color: #eee;
		border-radius: calc(20px);
		display: flex;
		flex-direction: column;
		justify-content: space-around;
		padding: 20px;
		font-size: 0.7rem;
		padding: 10px 10px;
	}
	.sendmes{
		background-color:#4682B4;
		width: 50%;
		padding: 20px;
		border-radius: calc(20px);
		font-size: 0.75rem;
		color: #eee;
		justify-content: flex-end;
		margin-left: 30vw;
	}
	.headwords{
		font-size: 35px;
		display: flex;
		justify-content: space-around;
		width: 100%;
		height: 20vh;
		flex-direction: column;
		}
	.uni-input {
		width: 200px;
		height: 30px;
		border-bottom: #F0AD4E solid 2px;
	}
	.pointbox{
		display: flex;
		align-items: center;
		justify-content: center;
		height: 1rem;
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
