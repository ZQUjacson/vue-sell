<template>
  <div class="header">
  	<div class="content-wrapper">
  		<div class="avatar">
  			<img width="64" height="64" :src="seller.avatar">
  		</div>
  		<div class="content">
  			<div class="title">
  				<span class="brand"></span>
  				<span class="name">{{seller.name}}</span>
  			</div>
  			<div class="description">
  				{{seller.description}}/{{seller.deliveryTime}}分钟送达
  			</div>
  			<div v-if="seller.supports" class="support">
  				<span class="icon" :class="Map[seller.supports[0].type]"></span>
  				<span class="text">{{seller.supports[0].description}}</span>
  			</div>
  		</div>
		<div v-if="seller.supports" class="support-count" @click="showDetail">
			<span class="count">{{seller.supports.length}}个</span>
			<i class="icon-keyboard_arrow_right"></i>
		</div>
  	</div>
  	<div class="bulletin-wrapper">
  		<span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
  		<i class="icon-keyboard_arrow_right"></i>
  	</div>
  	<div class="background">
  		<img :src="seller.avatar" width="100%" height="100%">
  	</div>
  	<div v-show="datailShow" class="detail" transition="fade">
  		<div class="detail-wrapper clearfix">
  			<div class="detail-main">
  				<h1 class="name">{{seller.name}}</h1>
  				<div class="star-wrapper">
  					<star :size="48" :score="seller.score"></star>
  				</div>
  				<div class="title">
  					<div class="line"></div>
  					<div class="text">优惠信息</div>
  					<div class="line"></div>
  				</div>
  				<ul v-if="seller.supports" class="supports">
  					<li class="support-item" v-for="item in seller.supports">
  						<span class="icon" :class="Map[seller.supports[$index].type]"></span>
  						<span class="text">{{seller.supports[$index].description}}</span>
  					</li>
  				</ul>
  				<div class="title">
  					<div class="line"></div>
  					<div class="text">商家公告</div>
  					<div class="line"></div>
  				</div>
  				<div class="bulletin">
  					<p class="content">{{seller.bulletin}}</p>
  				</div>
  			</div>
  		</div>
  		<div class="detail-close">
  			<i class="icon-close" @click="hideDetail"></i>
  		</div>
  	</div>
  </div>
</template>
<script type="text/ecmascript">
	import star from 'components/star/star.vue'

	export default {
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				datailShow: false
			}
		},
		methods: {
			showDetail() {
				this.datailShow = true
			},
			hideDetail() {
				this.datailShow = false
			}
		},
		created() {
			this.Map=['decrease','discount','guarantee','invoice','special']
		},
		components: {
			star
		}
	}
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/icon'
@import '../../common/stylus/base'

.header
	color:#fff
	position:relative
	background:rgba(7,17,27,.5)
	.content-wrapper
		position:relative
		padding:24px 12px 18px 24px
		font-size:0
		.avatar
			display:inline-block
			vertical-align:top
		.content
			display:inline-block
			margin-left:16px
			font-size:14px
			.title
				margin:2px 0 8px 0
				.brand
					display:inline-block
					vertical-align:top
					width:30px
					height:18px
					background-image:url('../../../resource/img/brand@2x.png')
					background-size:30px 18px
					background-repeat:no-repeat
				.name
					margin-left:6px
					font-size:16px
					line-height:18px
					font-weight:bold
			.description
				margin-bottom:10px
				line-height:12px
				font-weight:200
				font-size:12px
			.support
				.icon
					display:inline-block
					width:12px
					height:12px
					margin-right:4px
					background-size:12px
					&.decrease
						background-image:url('../../../resource/img/decrease_1@2x.png')
					&.discount
						background-image:url('../../../resource/img/discount_1@2x.png')
					&.guarantee
						background-image:url('../../../resource/img/guarantee_1@2x.png')
					&.invoice
						background-image:url('../../../resource/img/invoice_1@2x.png')
					&.special
						background-image:url('../../../resource/img/special_1@2x.png')
				.text
					vertical-align:top
					font-size:10px
					font-weight:200
					line-height:12px
		.support-count
			position:absolute
			right:12px
			bottom:18px
			padding:0 8px
			height:24px
			line-height:24px
			font-weight:200
			border-radius:14px
			background-color:rgba(0,0,0,0.2)
			text-align:center
			.count
				font-size:10px
			.icon-keyboard_arrow_right
				font-size:10px
	.bulletin-wrapper
		height:28px
		line-height:28px
		padding:0 22px 0 12px
		white-space:nowrap
		overflow:hidden
		text-overflow:ellipsis
		background-color:rgba(7,17,27,.2)
		.bulletin-title
			vertical-align:top
			display:inline-block
			width:22px
			height:12px
			background-image:url('../../../resource/img/bulletin@2x.png')
			background-repeat:no-repeat
			background-size:22px 12px
			margin-top:8px
		.bulletin-text
			vertical-align:top
			font-size:10px
			font-weight:200
			margin:0 4px
	.background
		position:absolute
		top:0
		left:0
		width:100%
		height:100%
		z-index:-1
		-webkit-filter:blur(10px)
	.detail
		position:fixed
		width:100%
		height:100%
		top:0
		left:0
		overflow:auto
		transition:all 0.5s
		background:rgba(7,17,27,0.8)
		z-index:100
		&.fade-transition
			opacity:1
			background:rbga(7,17,27,0.8)
		&.fade-enter,&.fade-leave
			opacity:0
			background:rbga(7,17,27,0)
		.detail-wrapper
			width:100%
			min-height:100%
			.detail-main
				margin-top:64px
				padding-bottom:64px
				&>h1
					font-size:16px
					font-weight:700
					text-align:center
					color:rgb(255,255,255)
					line-height:16px
				.star-wrapper
					margin-top:16px
					padding:2px 0
					text-align:center
				.title
					display:flex
					width:80%
					margin:30px auto 24px auto
					.line
						flex:1
						position:relative
						top:-6px
						border-bottom:1px solid rgba(255,255,255,0.2)
					.text
						padding:0 12px
						font-size:14px
						font-weight:200
				.bulletin
					width:80%
					margin:0 auto
					.content
						font-size:12px
						line-height:24px
						font-weight:200
		.detail-close
			position:relative
			width:32px
			height:32px
			margin:-64px auto 0 auto
			clear:both
			font-size:32px
	.supports
		width:80%
		margin:0 auto
		.support-item
			padding:0 12px
			margin-bottom:12px
			&:last-child
				margin-bottom:0
			.icon
				display:inline-block
				width:16px
				height:16px
				vertical-align:top
				margin-right:6px
				background-size:16px 16px
				background-repeat:no-repeat
				&.decrease
					background-image:url('../../../resource/img/decrease_1@2x.png')
				&.discount
					background-image:url('../../../resource/img/discount_1@2x.png')
				&.guarantee
					background-image:url('../../../resource/img/guarantee_1@2x.png')
				&.invoice
					background-image:url('../../../resource/img/invoice_1@2x.png')
				&.special
					background-image:url('../../../resource/img/special_1@2x.png')
			.text
				line-height:12px
				font-size:12px
				font-weight:200
</style>
