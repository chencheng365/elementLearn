<template>
	<header class="header">
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
            <icon :type="seller.supports[0].type" :iconNum=1></icon>
	          <span class="text">{{seller.supports[0].description}}</span>
	        </div>
	      </div>
	      <div v-if="seller.supports" class="support-count" @click="showDetail">
	        <span class="count">{{seller.supports.length}}个</span>
	        <i class="icon-keyboard_arrow_right"></i>
	      </div>
	    </div>
	    <div class="bulletin-wrapper" @click="showDetail">
	      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
	      <i class="icon-keyboard_arrow_right"></i>
	    </div>
	    <div class="background">
	      <img :src="seller.avatar" width="100%" height="100%">
	    </div>
	    <transition name="fade" mode="out-in">
	    	<div v-show="detailShow" class="detail">
		    	<div class="detail-wrapper clearfix">
		    		<div class="detail-main">
		    			<h1 class="name">
		    				{{seller.name}}
		    			</h1>
		    			<div class="star-warpper">
		    				<star :score="seller.score" :size="48"></star>
		    			</div>
						<div class="title">
							<div class="line"></div>
							<div class="text">优惠信息</div>
							<div class="line"></div>
						</div>
						<ul v-if='seller.supports' class="supports">
							<li class="support-item" v-for="(item,index) in seller.supports">
								<icon :type="seller.supports[index].type" :iconNum='2'></icon>
                <span class="text">{{seller.supports[index].description}}</span>
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
		    		<span class="icon-close" @click='hideDetail'></span>
		    	</div>
		    </div>
	    </transition>
	    
	  </header>
</template>

<script>
import star from '@/element/star/star'
import icon from '@/element/icon/icon'
export default {
	props: {
      seller: {
        type: Object
      }
    },
    components:{
    	star,
      icon
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    data() {
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail() {
        this.detailShow = true;
      },
      hideDetail() {
        this.detailShow = false;
      }
    },
}
</script>

<style lang='scss'>
@import '../../../assets/css/mixin.scss';

.header{
	position: relative;
    overflow: hidden;
    color: #fff;
    background: rgba(7, 17, 27, 0.5);

    .content-wrapper{
      position: relative;
      padding: 24px 12px 18px 24px;
      font-size: 0;
      .avatar{
        display: inline-block;
        vertical-align: top;
        img{border-radius: 2px}
      }
      .content{
      	display: inline-block;
      	margin-left: 16px;
      	.title{
      		margin: 2px 0px 8px 0px;
      		.brand{
      			display: inline-block;
      			vertical-align: top;
      			width:30px;
      			height: 18px;
				@include bg-image('brand');
				background-size: 30px 18px;
				background-repeat: no-repeat;
      		}
      		.name{
      			line-height: 18px;
      			margin-left: 6px;
      			font-size: 18px;
      			font-weight: bold;
      		}
      	}
      	.description{
      		margin-bottom: 10px;
      		line-height: 12px;
      		font-size: 12px;
      	}
      	.support{
      		.text{
	            line-height: 12px;
	            font-size: 12px;
	        }
      	}	
      }
      .support-count{
      	position: absolute;
      	right: 12px;
      	bottom: 14px;
      	height: 24px;
      	line-height: 24px;
      	border-radius: 14px;
      	background: rgba(0,0,0,0.2); 
      	padding: 0 8px;
      	.count{
          vertical-align: top;
          font-size: 11px;
        }
        .icon-keyboard_arrow_right{
          margin-left: 2px;
          font-size: 12px;
          line-height: 24px;
        }
      }
  	}

  	.bulletin-wrapper{
  		position: relative;
	    height: 28px;
	    line-height: 28px;
	    padding: 0 22px 0 12px;
	    white-space: nowrap;
	    overflow: hidden;
	    text-overflow: ellipsis;
	    background: rgba(7, 17, 27, 0.2);
	    .bulletin-title{
	    	display: inline-block;
	        vertical-align: baseline;
	        width: 22px;
	        height: 12px;
	        @include bg-image('bulletin');
	        background-size: 22px 12px;
	        background-repeat: no-repeat;
	    }
	    .bulletin-text{
	    	vertical-align: top;
	        margin: 0 4px;
	        font-size: 10px;
	    }
        .icon-keyboard_arrow_right{
        	position: absolute;
	        font-size: 10px;
	        right: 12px;
	        top: 8px;
        }
  	}
    
    .background{
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      filter: blur(10px);
    }

    .detail{
    	position: fixed;
    	top: 0;
    	left: 0;
    	z-index: 112;
    	width: 100%;
    	height: 100%;
    	overflow: auto;
    	background: rgba(7,17,27,.8);
    	transition:all 0.5s;
    	&.fade-enter-active, &.fade-leave-active{
    		opacity: 1;
    		background: rgba(7,17,27,.8);
    	}
    	&.fade-enter, &.fade-leave-active{
    		opacity: 0;
    		background: rgba(7,17,27,.0);
    	}
    	.detail-wrapper{
    		min-height: 100%;
    		width: 100%;
    		.detail-main{
    			margin-top: 64px;
    			padding-bottom: 64px;
    			.name{
    				line-height: 16px;
    				text-align: center;
    				font-size: 16px;
    				font-weight: 700;
    			}
    			.star-warpper{
    				margin-top: 18px;
    				padding:2px 0;
    				text-align: center;
    			}
    			.title{
    				display: flex;
    				width: 80%;
    				margin: 28px auto 24px auto;
    				.line{
    					flex: 1;
    					position: relative;
    					bottom: 6px;
    					border-bottom: 1px solid rgba(255,255,255,.2)
    				}
    				.text{
    					padding:0 12px;
    					font-weight: 700;
    					font-size: 14px;
    				}
    			}
    			.supports{
    				width: 80%;
    				margin:0 auto;
    				.support-item{
    					padding: 0 12px;
    					margin-bottom: 12px;
    					font-size: 0;
    					&:last-child{
							margin-bottom:0;
    					}
    					.text{
    						line-height: 12px;
    						font-size: 12px;
    					}
    				}
    			}
    			.bulletin{
    				width: 80%;
		            margin: 0 auto;
		            .content{
	            		padding: 0 12px;
	              		line-height: 24px;
	              		font-size: 12px;
		            }
    			}
	            
    		}
    	}
    	.detail-close{
    		position: relative;
    		width: 32px;
    		height: 32px;
    		margin:-64px auto 0 auto;
    		clear: both;
    		font-size: 32px;
    	}
    }  
}
</style>