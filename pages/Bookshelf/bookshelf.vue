<template>
	<view class="content">
		<view class="bookHead">
			<view class="SignIn">
				签到
			</view>
			<view class="title">
				闪电小说
			</view>
			<image src="../../static/Other.png" mode=""></image>
			<image src="../../static/History.png" mode=""></image>
			<image src="../../static/search.png" mode=""></image>
		</view>
		<view class="SignInbg">
			<view class="uni-padding-wrap uni-common-mt DayNumberProgress">
				<view class="progress-box">
					<progress :percent="ReadProgress" active stroke-width="6" activeColor="#f2a858" backgroundColor="rgb(215,215,215)"/>
				</view>
			</view>
			<view class="DayNumber">
				<view class="dayOne">
					<image src="../../static/siginno.png" mode=""></image>
					<view class="DayReward">
						<span>第一天</span>
						<span>(10)</span>
					</view>
				</view>
				<view class="daytow">
					<image src="../../static/siginno.png" mode=""></image>
					<view class="DayReward">
						<span>第五天</span>
						<span>(100)</span>
					</view>
				</view>
				<view class="dayThress">
					<image src="../../static/siginno.png" mode=""></image>
					<view class="DayReward">
						<span>第十天</span>
						<span>(300)</span>
					</view>
				</view>
			</view>
		</view>
		<view class="uni-swiper-msg Notice">
			<view class="uni-swiper-msg-icon NoticeImg">
				<image src="../../static/Notice.png" mode="widthFix"></image>
			</view>
			<swiper class="NoticeRight" vertical="true" autoplay="true" circular="true" interval="3000">
				<swiper-item v-for="(item, index) in msg" :key="index">
					<navigator class="NoticeContent">{{item}}</navigator>
				</swiper-item>
			</swiper>
		</view>
		<view class="BookList">
			<view class="BookItem" v-for="(Book,index) in BookArr" :key="index">
				<view class="BookItemProcess uni-media-list-logo">
					<image class="image" :class="{lazy:!Book.show}" :data-index="index" @load="onLoad" :src="Book.show?Book.thumb:''" />
					<image class="image placeholder" :class="{loaded:Book.loaded}" :src="placeholderSrc" />
					<view class="uni-padding-wrap uni-common-mt BookItemView">
						<view class="progress-box">
							<progress :percent="ReadProgress" active stroke-width="6" activeColor="#f2a858" backgroundColor="rgb(215,215,215)"/>
						</view>
					</view>
				</view>
				<view class="BookName">
					{{Book.title}}
				</view>
			</view>
			<view class="BookItem">
				<view class="BookItemProcess lastBook">
					<image class="BookMore" src="../../static/more.png" mode=""></image>
					<view class="uni-padding-wrap uni-common-mt BookItemView">
						<view class="progress-box">
							<progress :percent="ReadProgress" active stroke-width="6" activeColor="#f2a858" backgroundColor="rgb(215,215,215)"/>
						</view>
					</view>
				</view>
				<view class="BookName">
					&nbsp;
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				ReadProgress:"0",
				msg : [
					'闪电小说很快就会跟大家见面了！',
					'创意魔方完成B2轮融资，闪电小说震撼发布！'
				],
				BookArr:[],
				windowHeight: 0,
				placeholderSrc: '../../static/logoLoading.png',
				show: false
			}
		},
		methods: {
			initData(){
				let that =this;
				uni.request({
					url:that.$serverUrl +'/api/v1/book/list?start=0&limit=5', 
					dataType:'json', //默认 json格式
					data:{
						//返回参数
					},
					method:'GET',  //请求方式
					success:(res) => {
						console.log(res)
						// that.BookArr=res.data.data.books;
						for (var i=0;i<res.data.data.books.length;i++) {
							res.data.data.books[i].show = "false";
							res.data.data.books[i].loaded = "false";
							that.BookArr.push(res.data.data.books[i])
						}
						console.log(that.BookArr)
					},
					fail:(error) => {
						console.log(error)
					}
				})
			},
			load() {
				uni.createSelectorQuery().selectAll('.lazy').boundingClientRect((images) => {
					images.forEach((image, index) => {
						if (image.top <= this.windowHeight) {
							this.BookArr[image.dataset.index].show = true;
						}
					})
				}).exec()
			},
			onLoad(e) {
				this.BookArr[e.target.dataset.index].loaded = true
			}
		},
		onLoad() {
			this.initData();
			this.windowHeight = uni.getSystemInfoSync().windowHeight;
		},
		onShow() {
			if (!this.show) {
				this.show = true
				setTimeout(() => {
					this.load()
				}, 100)
			}
		},
		onPageScroll() {
			this.load()
		}
	}
</script>

<style>
	.bookHead {
		text-align: center;
		height: 197.91upx;
		background: rgb(251,148,41);
		padding: 0 31.25upx;
		position: relative;
	}
	.bookHead image{
		float: right;
		display: inline-block;
		width: 37.5upx;
		height: 37.5upx;
		margin: 26.04upx 0upx 0 20.83upx;
		
	}
	.title {
		font-size: 36.45upx;
		color: white;
		height: 93.75upx;
		line-height: 93.75upx;
		display: inline-block;
		position: absolute;
		width: 166.66upx;
		left: 50%;
		margin-left: -83.33upx;
		text-align: center;
	}
	.SignIn{
		font-size: 21.87upx;
		display: inline-block;
		float: left;
		background: white;
		padding: 0 15.62upx;
		border-radius: 8.33upx;
		height: 41.66upx;
		margin-top: 26.04upx;
		line-height: 41.66upx;
	}
	.SignInbg{
		margin: 0 31.25upx;
		width: 687.5upx;
		height: 197.91upx;
		margin-top: -98.95upx;
		position: relative;
		background: url(../../static/Signin.png) no-repeat;
		background-size: 100% 100%;
	}
	.DayNumber{
		display: flex;
		justify-content: space-around;
		align-items: center;
		height: 197.91upx;
	}
	.DayNumberProgress{
		position: absolute;
		top: 50%;
		z-index: 99;
		width: 562.5upx;
		height: 6.25upx;
		margin-top: -3.12upx;
		margin-left: 62.5upx;
		border-radius: 8.33upx;
	}
	.progress-box,.uni-progress{
		width: 100%;
		background-color: rgb(216,216,216);
	}
	.dayOne image,.daytow image,.dayThress image{
		width: 52.08upx;
		height: 52.08upx;
		z-index: 99;
		margin-top:23.95upx;
	}
	.DayReward{
		text-align: center;
	}
	.DayReward span{
		font-size: 20.83upx;
		/* color: #f2a858; */
		color: #abaaaa;
		display: block;
	}
	.Notice{
		width: 687.5upx;
		margin: 10.41upx 31.25upx;
		height: 53.12upx;
		background: #fafafa;
		border-radius: 8.33upx;
	}
	.NoticeImg{
		display: inline-block;
	}
	.NoticeImg image{
		width: 32.29upx;
		height: 32.29upx;
		margin: 10.41upx 20.83upx;
	}
	.NoticeRight{
		display: inline-block;
		width: 604.16upx;
		height: 53.12upx;
	}
	.NoticeContent{
		font-size: 26.04upx;
		line-height: 53.12upx;
	}
	.BookList{
		margin: 10.41upx 41.66upx;
	}
	.BookItem{
		width: 172.91upx;
		height: 291.66upx;
		margin-right: 75upx;
		margin-top: 20.83upx;
		display: inline-block;
	}
	.BookItem:nth-child(3n){
		margin-right: 0upx;
	}
	.BookItemProcess{
		width: 172.91upx;
		height: 237.5upx;
		position: relative;
		border-bottom: 8.33upx;
		box-shadow: 2px 2px 20px #ccc;
	}
	.BookItemProcess image{
		width: 100%;
		height: 100%;
		border-bottom: 8.33upx;
	}
	.BookItem:last-child{
		vertical-align: top;
	}
	.lastBook{
		border: 1px solid #cbcbcb;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.BookMore{
		width: 31.25upx !important;
		height: 31.25upx !important;
	}
	.BookItemView{
		position: absolute;
		bottom: 0;
		width: 100%;
	}
	.BookName{
		width: 172.91upx;
		font-size: 29.16upx;
		margin: 10.41upx 0;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}
	
	
	
	
	
	
	.placeholder {
		opacity: 1;
		transition: opacity 0.4s linear;
	}
	
	.placeholder.loaded {
		opacity: 0;
	}
	
	.uni-media-list-logo {
		position: relative;
	}
	
	.uni-media-list-logo .image {
		position: absolute;
	}
</style>
