<template>
	<div class="goods">
		<div class="menu-wrapper" v-el:menu-wrapper>
			<ul>
				<li v-for="item in goods" class="menu-item" :class="{'current':currentIndex===$index}" @click="selectMenu($index,$event)">
					<span class="text"><span v-show="item.type>0" class="icon" :class="Map[item.type]"></span>{{item.name}}</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrpper" v-el:foods-wrapper>
			<ul>
				<li v-for="item in goods" class="food-list food-list-hook">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li v-for="food in item.foods" class="food-item">
							<div class="icon">
								<img width="57px" height="57px" :src="food.icon">
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<div class="extra">
									<span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
								</div>
								<div class="cartcontral-wrapper">
									<cartcontral :food="food"></cartcontral>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
		<shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
	</div>
</template>
<script>
	import BScroll from 'better-scroll'
	import shopcart from 'components/shopcart/shopcart'
	import cartcontral from 'components/cartcontral/cartcontral'

	const ERR_OK = 0

	export default {
		props: {
			seller: {
				type: Object
			}
		},
		created() {
			this.Map = ['decrease', 'discount', 'guarantee', 'invoice', 'special']

			this.$http.get('api/goods').then((response) => {
				response = response.body
				if (response.errno === ERR_OK) {
					this.goods = response.goods
					console.log(this.goods)
					this.$nextTick(() => {
						this._initScroll()
						this._calculateHeight()
					})
				}
			})
		},
		data() {
			return {
				goods: [],
				listHeight: [],
				scrollY: 0
			}
		},
		computed: {
			currentIndex() {
				for (var i = 0; i < this.listHeight.length; i++) {
					let height1 = this.listHeight[i]
					let height2 = this.listHeight[i + 1]
					if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
						return i
					}
				}
			},
			selectFoods() {
				let foods = []
				this.goods.forEach((good) => {
					good.foods.forEach((food) => {
						if (food.count) {
							foods.push(food)
						}
					})
				})
				return foods
			}
		},
		methods: {
			_initScroll() {
				this.menuScroll = new BScroll(this.$els.menuWrapper, {
					click: true
				})
				this.foodScroll = new BScroll(this.$els.foodsWrapper, {
					click: true,
					probeType: 3
				})
				this.foodScroll.on('scroll', (pos) => {
					this.scrollY = Math.abs(Math.round(pos.y))
				})
			},
			_calculateHeight() {
				let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook')
				let height = 0
				this.listHeight.push(height)
				for (let i = 0; i < foodList.length; i++) {
					let item = foodList[i]
					height += item.clientHeight
					this.listHeight.push(height)
				}
			},
			selectMenu(index, event) {
				if (!event._constructed) {
					return
				}
				let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook')
				let el = foodList[index]
				this.foodScroll.scrollToElement(el, 300)
			}
		},
		components: {
			shopcart,
			cartcontral
		}
	}
</script>
<style lang="stylus" rel="stylesheet/stylus">
.goods
	display:flex
	position:absolute
	top:174px
	bottom:46px
	width:100%
	overflow:hidden
	.menu-wrapper
		flex:0 0 80px
		width:80px
		background-color:#f3f5f7
		.menu-item
			display:table
			width:56px
			height:54px
			line-height:14px
			font-size:12px
			padding:0 12px
			&.current
				position:relative
				z-indez:10
				margin-top:-1px
				background:#fff
				font-weight:700
			.icon
				display:inline-block
				width:12px
				height:12px
				margin-right:2px
				background-size:12px
				&.decrease
					background-image:url('../../../resource/img/decrease_3@2x.png')
				&.discount
					background-image:url('../../../resource/img/discount_3@2x.png')
				&.guarantee
					background-image:url('../../../resource/img/guarantee_3@2x.png')
				&.invoice
					background-image:url('../../../resource/img/invoice_3@2x.png')
				&.special
					background-image:url('../../../resource/img/special_3@2x.png')
			.text
				display:table-cell
				font-size:12px
				width:56px
				vertical-align:middle
	.foods-wrpper
		flex:1
		.title
			padding-left:14px
			font-size:12px
			height:26px
			line-height:26px
			border-left:2px solid #d9dde1
			background-color:#f3f5f7
			color:rgb(147,153,159)
		.food-item
			display:flex
			margin:18px
			padding-bottom:18px
			.icon
				flex:0 0 57px
				margin-right:10px
			.content
				flex:1
				position:relative
				.name
					margin:2px 0 8px 0
					height:14px
					line-height:14px
					font-size:14px
					color:rgb(7,17,27)
				.desc, .extra
					font-size:10px
					line-height:10px
					color:rgb(147,153,159)
				.desc
					line-height:12px
					margin-bottom:8px
				.extra
					.count
						margin-right:12px
				.price
					font-weight:700
					line-height:24px
					.now
						margin-right:12px
						font-size:14px
						color:rgb(240,20,20)
					.old
						line-decoration:line-through
						font-size:10px
						color:rgb(147,153,159)
				.cartcontral-wrapper
					position: absolute
					right: 0
					bottom: 0

</style>