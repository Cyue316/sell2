<template>
	<div class="shopcart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'highlight':totalCount>0}">
						<i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
					</div>
					<div class="num" v-show="totalCount>0">{{totalCount}}</div>
				</div>
				<div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
				<div class="desc">另需配送费￥{{deliveryprice}}元</div>
			</div>
			<div class="content-right">
				<div class="pay" :class="payClass">{{payDesc}}</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		props:{
			selectfoods:{
				type:Array,
				default(){
					return [];
				}
			},
			deliveryprice:{
				type:Number,
				default:0
			},
			minprice:{
				type:Number,
				default:0
			}
		},
		computed:{
			totalPrice: function(){
				let total = 0;
				this.selectfoods.forEach((food,index) => {
					total += food.price * food.count;
				});
				return total;
			},
			totalCount: function(){
				let count = 0;
				this.selectfoods.forEach((food,index) => {
					count += food.count;
				});
				return count;
			},
			payDesc: function(){
				if(this.totalPrice === 0 ){
					return `￥${this.minprice}元起送`;
				}else if(this.totalPrice < this.minprice){
					let diff = this.minprice - this.totalPrice;
					return `还差￥${diff}元起送`;
				}else{
					return '去结算';
				}
			},
			payClass:function(){
				if(this.totalPrice > this.minprice){
					return 'enough';
				}else{
					return 'not-enough';
				}
			}
		}
	};
</script>

<style lang="scss" rel="stylesheet/scss">
	.shopcart{
		position:fixed;
		left:0;
		bottom:0;
		z-index:50;
		width: 100%;
		height:48px;
		.content{
			display:flex;
			background:#141d27;
			font-size: 0;
			.content-left{
				flex:1;
				.logo-wrapper{
					display:inline-block;
					vertical-align: top;
					position:relative;
					top:-10px;
					margin:0 12px;
					padding:6px;
					width:56px;
					height:56px;
					box-sizing: border-box;
					border-radius: 50%;
					background:#141d27;
					.logo{
						width:100%;
						height:100%;
						text-align: center;
						border-radius:50%;
						background:rgba(255,255,255,0.2);
						&.highlight{
							background:rgb(0,160,220);
						}
						.icon-shopping_cart{
							line-height: 44px;
							font-size:24px;
							color:rgba(255,255,255,0.4);
							&.highlight{
								color:#fff;
							}
						}
					}
					.num{
						position:absolute;
						top:0;
						right:0;
						width:24px;
						height:16px;
						line-height:16px;
						text-align:center;
						border-radius:10px;
						font-size:9px;
						font-weight:700;
						color:#fff;
						background:rgb(240,20,20);
						box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
					}
				}
				.price{
					display:inline-block;
					vertical-align:top;
					line-height:24px;
					margin-top:12px;
					padding-right:12px;
					box-sizing: border-box;
					border-right:1px solid rgba(255,255,255,0.1);
					font-size:16px;
					font-weight: 700;
					color:rgba(255,255,255,0.4);
					&.highlight{
						color:#fff;
					}
				}
				.desc{
					display:inline-block;
					vertical-align:top;
					line-height:24px;
					margin:12px 0 0 12px;
					font-size:10px;
					color:rgba(255,255,255,0.4);
				}
			}
			.content-right{
				flex:0 0 105px;
				width:105px;
				.pay{
					height:48px;
					line-height:48px;
					text-align:center;
					color:rgba(255,255,255,0.4);
					font-weight:700;
					font-size:12px;
					background:#2b333b;
					&.not-enough{
						background:#2b333b;
					}
					&.enough{
						background:#00b43c;
						color:#fff;
					}
				}
			}
		}
	}
</style>