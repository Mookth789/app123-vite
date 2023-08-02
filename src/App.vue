<script type="module">
		const app = Vue.createApp({
			data() {
				return {
					globalData: { test: '1' },
					search1: '',
					tabsDatas: [
						{ text: `首页1`, icon: `` },
						{ text: `课程`, icon: `` },
						{ text: `听书`, icon: `` },
						{ text: `电子书`, icon: `` },
						{ text: `训练营`, icon: `` },
						{ text: `其他`, icon: `` }
					],
					tabsTimer: null,
					tabsMainIndex: 0,
					tabsLeft: 0,
					tabsWidth: 0,
					tabsItemWidth: 0,
					tabsIndex: 0
				};
			},
			created() {
				this.init();
			},
			methods: {
				async init() {}, // 获取一个目标元素的高度
				getElTabsRect(elClass, dataVal) {
					new Promise((resolve, reject) => {
						const query = uni.createSelectorQuery().in(this);
						query
							.select('.' + elClass)
							.fields(
								{
									size: true
								},
								(res) => {
									// 如果节点尚未生成，res值为null，循环调用执行
									if (!res) {
										setTimeout(() => {
											this.getElTabsRect(elClass);
										}, 10);
										return;
									}
									this[dataVal] = res.width;
									resolve();
								}
							)
							.exec();
					});
				},
				// 设置左边菜单的滚动状态
				async setTabsStatus(index) {
					this.current = index;
					// 如果为0，意味着尚未初始化
					if (this.tabsWidth == 0 || this.tabsItemWidth == 0) {
						await this.getElTabsRect('tabs-title', 'tabsWidth');
						await this.getElTabsRect('tabs-item-title', 'tabsItemWidth');
					}
					// 将菜单活动item垂直居中
					this.tabsLeft = index * this.tabsItemWidth + this.tabsItemWidth / 2 - this.tabsWidth / 2;
				},
				changeTabs(evt) {
					let { index } = evt.currentTarget.dataset;
					if (index == this.tabsIndex) return;
					this.setData({
						tabsIndex: index
					});
					this.setTabsStatus(index);
				},
				scrolltoupperFlex5(evt) {},
				scrolltolowerFlex5(evt) {}
			}
		});
		app.use(vant);
		app.use(DiygwLib);
		app.config.globalProperties.$tools = new Tools();
		app.config.globalProperties.$http = httpService;
		app.config.globalProperties.$session = Session;
		app.mixin({
			methods: {
				Validate,
				setData,
				navigateTo,
				showModal,
				showToast,
				uploadImage
			}
		});
		app.mount('#app');
	</script>

<template>

  			<div class="container container23496">
				<div class="flex diygw-col-24 justify-between items-center flex-nowrap flex-clz">
					<div class="diygw-col-19">
						<div class="diygw-search">
							<div class="flex1 align-center flex padding-xs solid radius">
								<span style="color: #555 !important" class="diy-icon-search"></span>
								<van-field class="flex1" name="search1" type="" v-model="search1" placeholder="请输入关键字"></van-field>
							</div>
						</div>
					</div>
					<div class="flex diygw-col-0">
						<div class="diygw-avatar avatar-avatar xs radius bg-none">
							<img mode="aspectFit" class="diygw-avatar-img radius" src="./static/icon15_yj.png" />
							<div class="diygw-tag badge avatar-badge-0 red">1</div>
						</div>
					</div>
					<div class="flex diygw-col-0">
						<div class="diygw-tag margin-xs radius">
							<img class="diygw-tag-img" src="./static/icon8_qd.png" />
							<span> 签到 </span>
						</div>
					</div>
				</div>
				<div class="flex diygw-col-24">
					<van-notice-bar class="flex1 diy-notice-bar" scrollable color="#07c160" background="#fff" left-icon=" diy-icon-notification">
						<span class="diy-notice-item"> DIY官网做最好的拖拽式可视化设计工具 </span>
					</van-notice-bar>
				</div>
				<div class="flex diygw-col-24 justify-between items-center flex-nowrap flex1-clz">
					<div class="flex diygw-col-20 flex-direction-column">
						<scroll-view scroll-x :show-scrollbar="false" enhanced :scroll-into-view="'scroll-' + tabsIndex" :scroll-left="tabsLeft" scroll-with-animation class="diygw-tabs text-center justify-start scale-title small-border tabs-title">
							<div :id="'scroll-' + index" class="diygw-tab-item tabs-item-title" :class="index==tabsIndex?' cur text-black':''" v-for="(item,index) in  tabsDatas" :key="index" @click="changeTabs" :data-index="index"><span v-if="item.icon" :class="item.icon"></span> {{item.text}}</div>
						</scroll-view>
					</div>
					<div class="flex diygw-col-0">
						<div class="diygw-avatar avatar2-avatar md white radius">
							<span class="diy-icon-sort"></span>
						</div>
					</div>
				</div>
				<div class="flex diygw-col-24 swiper-clz">
					<van-swipe class="swiper swiper-indicator_rect" indicator-color="rgba(51, 51, 51, 0.39)" indicator-active-color="#fff" indicator-dots="true" autoplay="3000" interval="3000" circular="true" style="height: 150px">
						<van-swipe-item class="diygw-swiper-item">
							<div class="diygw-swiper-item-wrap">
								<img src="./static/pic1.jpg" class="diygw-swiper-image" />
							</div>
						</van-swipe-item>
						<van-swipe-item class="diygw-swiper-item">
							<div class="diygw-swiper-item-wrap">
								<img src="./static/pic2.jpg" class="diygw-swiper-image" />
							</div>
						</van-swipe-item>
						<van-swipe-item class="diygw-swiper-item">
							<div class="diygw-swiper-item-wrap">
								<img src="./static/pic3.jpg" class="diygw-swiper-image" />
							</div>
						</van-swipe-item>
					</van-swipe>
				</div>
				<div class="flex diygw-col-24">
					<div class="diygw-grid col-5">
						<div @click="navigateTo" data-type="openmodal" data-id="grid-modal" id="grid-modal" class="diygw-grid-item">
							<div class="diygw-grid-inner grid-item-clz">
								<div class="diygw-grid-title">热门</div>
							</div>
						</div>
						<div class="diygw-grid-item">
							<div class="diygw-grid-inner grid-item-clz">
								<div class="diygw-grid-title">法律</div>
							</div>
						</div>
						<div class="diygw-grid-item">
							<div class="diygw-grid-inner grid-item-clz">
								<div class="diygw-grid-title">营销</div>
							</div>
						</div>
						<div class="diygw-grid-item">
							<div class="diygw-grid-inner grid-item-clz">
								<div class="diygw-grid-title">文学</div>
							</div>
						</div>
						<div class="diygw-grid-item">
							<div class="diygw-grid-inner grid-item-clz">
								<div class="diygw-grid-title">排行</div>
							</div>
						</div>
						<div class="diygw-grid-item">
							<div class="diygw-grid-inner grid-item-clz">
								<div class="diygw-grid-title">教育</div>
							</div>
						</div>
						<div class="diygw-grid-item">
							<div class="diygw-grid-inner grid-item-clz">
								<div class="diygw-grid-title">商场</div>
							</div>
						</div>
						<div class="diygw-grid-item">
							<div class="diygw-grid-inner grid-item-clz">
								<div class="diygw-grid-title">安全</div>
							</div>
						</div>
						<div class="diygw-grid-item">
							<div class="diygw-grid-inner grid-item-clz">
								<div class="diygw-grid-title">提升</div>
							</div>
						</div>
						<div class="diygw-grid-item">
							<div class="diygw-grid-inner grid-item-clz">
								<div class="diygw-grid-title">客服</div>
							</div>
						</div>
					</div>
				</div>
				<div class="flex diygw-col-24 line-clz">
					<div class="diygw-pzx" style="border-bottom: 1px solid #eee"></div>
				</div>
				<div class="diygw-title flex diygw-col-24">
					<div class="title font-normal">好知识免费领</div>
				</div>
				<div class="flex flex-wrap diygw-col-24">
					<div class="flex diygw-col-12 flex-direction-column justify-center items-center flex-nowrap flex3-clz">
						<img src="./static/wbt-3jpg" class="image1-size diygw-image diygw-col-24 image1-clz" mode="widthFix" />
						<div class="diygw-col-0 text-clz">DIY视频教程</div>
						<div class="diygw-col-0 text1-clz">免费领取</div>
					</div>
					<div class="flex diygw-col-12 flex-direction-column justify-center items-center flex-nowrap flex4-clz">
						<img src="./static/d3ad3a6bf322e6b7d8c6a7fa09e80f26.jpg" class="image3-size diygw-image diygw-col-24 image3-clz" mode="widthFix" />
						<div class="diygw-col-0 text2-clz">DIY视频教程</div>
						<div class="diygw-col-0 text3-clz">免费领取</div>
					</div>
				</div>
				<div class="diygw-title flex diygw-col-24">
					<div class="title font-normal">榜单</div>
				</div>
				<scroll-view @scrolltoupper="scrolltoupperFlex5" @scrolltolower="scrolltolowerFlex5" scroll-x scroll-with-animation class="flex scroll-view diygw-col-24 flex-nowrap flex5-clz">
					<div class="flex flex-nowrap">
						<div class="flex flex-wrap diygw-col-21 flex-direction-column flex7-clz">
							<div class="diygw-title flex gradual-orange diygw-col-24">
								<div class="title font-normal">热门课程Top50</div>
								<div class="more">
									<span class="diy-icon-right"></span>
								</div>
							</div>
							<div class="flex diygw-col-24">
								<div class="diygw-list">
									<div style="" class="diygw-item col-100 row arrow solid-bottom list1-item-clz">
										<div class="diygw-avatar list1-icon-clz">
											<img mode="aspectFit" class="diygw-avatar-img" src="./static/wbt-4jpg" />
										</div>
										<div class="content">
											<div class="title">设计新人【实战DIY可视化】</div>
											<div class="text-sm remark">教大家如何快速用DIY可视化做项目</div>
										</div>
									</div>
									<div style="" class="diygw-item col-100 row arrow solid-bottom list1-item-clz">
										<div class="diygw-avatar list1-icon-clz">
											<img mode="aspectFit" class="diygw-avatar-img" src="./static/wbt-3jpg" />
										</div>
										<div class="content">
											<div class="title">设计新人【实战DIY可视化】</div>
											<div class="text-sm remark">教大家如何快速用DIY可视化做项目</div>
										</div>
									</div>
									<div style="" class="diygw-item col-100 row arrow solid-bottom list1-item-clz">
										<div class="diygw-avatar list1-icon-clz">
											<img mode="aspectFit" class="diygw-avatar-img" src="./static/wbt-4jpg" />
										</div>
										<div class="content">
											<div class="title">设计新人【实战DIY可视化】</div>
											<div class="text-sm remark">教大家如何快速用DIY可视化做项目</div>
										</div>
									</div>
								</div>
							</div>
						</div>
						<div class="flex flex-wrap diygw-col-21 flex-direction-column flex11-clz">
							<div class="diygw-title flex gradual-red diygw-col-24">
								<div class="title font-normal">热门课程Top50</div>
								<div class="more">
									<span class="diy-icon-right"></span>
								</div>
							</div>
							<div class="flex diygw-col-24">
								<div class="diygw-list">
									<div style="" class="diygw-item col-100 row arrow solid-bottom list6-item-clz">
										<div class="diygw-avatar list6-icon-clz">
											<img mode="aspectFit" class="diygw-avatar-img" src="./static/wbt-4jpg" />
										</div>
										<div class="content">
											<div class="title">设计新人【实战DIY可视化】</div>
											<div class="text-sm remark">教大家如何快速用DIY可视化做项目</div>
										</div>
									</div>
									<div style="" class="diygw-item col-100 row arrow solid-bottom list6-item-clz">
										<div class="diygw-avatar list6-icon-clz">
											<img mode="aspectFit" class="diygw-avatar-img" src="./static/wbt-3jpg" />
										</div>
										<div class="content">
											<div class="title">设计新人【实战DIY可视化】</div>
											<div class="text-sm remark">教大家如何快速用DIY可视化做项目</div>
										</div>
									</div>
									<div style="" class="diygw-item col-100 row arrow solid-bottom list6-item-clz">
										<div class="diygw-avatar list6-icon-clz">
											<img mode="aspectFit" class="diygw-avatar-img" src="./static/wbt-4jpg" />
										</div>
										<div class="content">
											<div class="title">设计新人【实战DIY可视化】</div>
											<div class="text-sm remark">教大家如何快速用DIY可视化做项目</div>
										</div>
									</div>
								</div>
							</div>
						</div>
						<div class="flex flex-wrap diygw-col-3 flex-direction-column"></div>
					</div>
				</scroll-view>
				<div class="flex diygw-col-24">
					<div class="diygw-pzx" style="border-bottom: 5px solid #eee"></div>
				</div>
				<div class="flex flex-wrap diygw-col-24 flex-direction-column">
					<div class="flex diygw-col-24">
						<div class="diygw-list">
							<div style="" class="diygw-item col-100 row solid-bottom">
								<div class="diygw-avatar">
									<img mode="aspectFit" class="diygw-avatar-img" src="./static/icon1_rm.png" />
								</div>
								<div class="content">
									<div class="title">DIY可视化</div>
									<div class="text-sm remark">3天前 DIY可视化公司</div>
								</div>
							</div>
						</div>
					</div>
					<div class="flex diygw-col-24">
						<div class="diygw-list">
							<div style="" class="diygw-item col-100 flex-direction-row-reverse solid-bottom list4-item-clz">
								<div class="diygw-avatar list4-icon-clz">
									<img mode="aspectFit" class="diygw-avatar-img" src="./static/miniprogramcode_177png" />
								</div>
								<div class="content">
									<div class="title">DIY可视化开源推荐</div>
									<div class="text-sm remark">DIY官网可视化工具做好的可视化拖拽开发工具，无须编程、零代码基础、所见即所得设计工具，轻松制作微信小程序、支付宝小程序、Vue3、H5、WebApp、UNIAPP、单页动画</div>
								</div>
							</div>
						</div>
					</div>
					<div class="flex diygw-col-24 justify-between">
						<div class="diygw-tag md radius">
							<span class="diygw-icon diy-icon-share"></span>
							<span> 1000 </span>
						</div>
						<div class="diygw-tag md radius">
							<span class="diygw-icon diy-icon-comment"></span>
							<span> 1000 </span>
						</div>
						<div class="diygw-tag md radius">
							<span class="diygw-icon diy-icon-appreciate"></span>
							<span> 2020-2-12 </span>
						</div>
					</div>
					<div class="flex diygw-col-24">
						<div class="diygw-pzx" style="border-bottom: 5px solid #eee"></div>
					</div>
				</div>
				<div class="flex flex-wrap diygw-col-24 flex-direction-column">
					<div class="flex diygw-col-24">
						<div class="diygw-list">
							<div style="" class="diygw-item col-100 row solid-bottom">
								<div class="diygw-avatar">
									<img mode="aspectFit" class="diygw-avatar-img" src="./static/icon1_rm.png" />
								</div>
								<div class="content">
									<div class="title">DIY可视化</div>
									<div class="text-sm remark">3天前 DIY可视化公司</div>
								</div>
							</div>
						</div>
					</div>
					<div class="diygw-col-24 text5-clz">DIY官网可视化工具做好的可视化拖拽开发工具，无须编程、零代码基础、所见即所得设计工具，轻松制作微信小程序、支付宝小程序、Vue3、H5、WebApp、UNIAPP、单页动画</div>
					<div class="flex diygw-col-24 items-center flex-nowrap">
						<img src="./static/diygwjpg" class="image-size diygw-image diygw-col-0 image-clz" mode="widthFix" />
					</div>
					<div class="flex diygw-col-24 justify-between">
						<div class="diygw-tag md radius">
							<span class="diygw-icon diy-icon-share"></span>
							<span> 1000 </span>
						</div>
						<div class="diygw-tag md radius">
							<span class="diygw-icon diy-icon-comment"></span>
							<span> 1000 </span>
						</div>
						<div class="diygw-tag md radius">
							<span class="diygw-icon diy-icon-appreciate"></span>
							<span> 2020-2-12 </span>
						</div>
					</div>
					<div class="flex diygw-col-24">
						<div class="diygw-pzx" style="border-bottom: 5px solid #eee"></div>
					</div>
				</div>
				<div class="flex flex-wrap diygw-col-24 flex-direction-column">
					<div class="flex diygw-col-24">
						<div class="diygw-list">
							<div style="" class="diygw-item col-100 row solid-bottom">
								<div class="diygw-avatar">
									<img mode="aspectFit" class="diygw-avatar-img" src="./static/icon1_rm.png" />
								</div>
								<div class="content">
									<div class="title">DIY可视化</div>
									<div class="text-sm remark">3天前 DIY可视化公司</div>
								</div>
							</div>
						</div>
					</div>
					<div class="diygw-col-24 text6-clz">DIY官网可视化工具做好的可视化拖拽开发工具，无须编程、零代码基础、所见即所得设计工具，轻松制作微信小程序、支付宝小程序、Vue3、H5、WebApp、UNIAPP、单页动画</div>
					<div class="flex diygw-col-24 items-center flex-nowrap">
						<img src="./static/pic1.jpg" class="image4-size diygw-image diygw-col-0 image4-clz" mode="widthFix" />
						<img src="./static/diygwjpg" class="image6-size diygw-image diygw-col-0 image6-clz" mode="widthFix" />
					</div>
					<div class="flex diygw-col-24 justify-between">
						<div class="diygw-tag md radius">
							<span class="diygw-icon diy-icon-share"></span>
							<span> 1000 </span>
						</div>
						<div class="diygw-tag md radius">
							<span class="diygw-icon diy-icon-comment"></span>
							<span> 1000 </span>
						</div>
						<div class="diygw-tag md radius">
							<span class="diygw-icon diy-icon-appreciate"></span>
							<span> 2020-2-12 </span>
						</div>
					</div>
					<div class="flex diygw-col-24">
						<div class="diygw-pzx" style="border-bottom: 5px solid #eee"></div>
					</div>
				</div>
				<div class="version text-grey flex-direction-column flex diygw-col-24 justify-center align-center">
				</div>
				<div class="clearfix"></div>
			</div>

      


</template>



<style scoped>
		.flex-clz {
			margin-left: 10px;
			z-index: 1000;
			width: calc(100% - 10px - 10px) !important;
			margin-top: 10px;
			margin-bottom: 0px;
			margin-right: 10px;
		}
		.avatar-avatar {
			border: solid;
		}
		.avatar-badge-0 {
			right: -5px;
		}
		.flex1-clz {
			margin-left: 10px;
			z-index: 1000;
			width: calc(100% - 10px - 10px) !important;
			margin-top: 0px;
			margin-bottom: 10px;
			margin-right: 10px;
		}
		.avatar2-avatar {
			border: solid;
		}
		.swiper-clz {
			margin-left: 10px;
			border-bottom-left-radius: 6px;
			overflow: hidden;
			width: calc(100% - 10px - 10px) !important;
			border-top-left-radius: 6px;
			margin-top: 0px;
			border-top-right-radius: 6px;
			border-bottom-right-radius: 6px;
			margin-bottom: 5px;
			margin-right: 10px;
		}
		.line-clz {
			margin-left: 10px;
			width: calc(100% - 10px - 10px) !important;
			margin-top: 0px;
			margin-bottom: 0px;
			margin-right: 10px;
		}
		.flex3-clz {
			margin-left: 5px;
			border-bottom-left-radius: 6px;
			box-shadow: 0px 1px 3px rgba(31, 31, 31, 0.16);
			overflow: hidden;
			width: calc(50% - 5px - 5px) !important;
			border-top-left-radius: 6px;
			margin-top: 5px;
			border-top-right-radius: 6px;
			border-bottom-right-radius: 6px;
			margin-bottom: 5px;
			margin-right: 5px;
		}
		.image1-clz {
			margin-left: 8px;
			border-bottom-left-radius: 10px;
			overflow: hidden;
			width: calc(100% - 8px - 8px) !important;
			border-top-left-radius: 10px;
			margin-top: 8px;
			border-top-right-radius: 10px;
			border-bottom-right-radius: 10px;
			margin-bottom: 8px;
			margin-right: 8px;
		}
		.image1-size {
			height: 80px !important;
			width: 100%;
		}
		.text-clz {
			font-size: 16px !important;
		}
		.text1-clz {
			border: 1px solid #fb0909;
			padding-top: 3px;
			border-bottom-left-radius: 30px;
			color: #fb0909;
			padding-left: 15px;
			font-size: 16px !important;
			padding-bottom: 3px;
			border-top-right-radius: 30px;
			margin-right: 5px;
			margin-left: 5px;
			overflow: hidden;
			border-top-left-radius: 30px;
			margin-top: 5px;
			border-bottom-right-radius: 30px;
			margin-bottom: 5px;
			padding-right: 15px;
		}
		.flex4-clz {
			margin-left: 5px;
			border-bottom-left-radius: 6px;
			box-shadow: 0px 1px 3px rgba(31, 31, 31, 0.16);
			overflow: hidden;
			width: calc(50% - 5px - 5px) !important;
			border-top-left-radius: 6px;
			margin-top: 5px;
			border-top-right-radius: 6px;
			border-bottom-right-radius: 6px;
			margin-bottom: 5px;
			margin-right: 5px;
		}
		.image3-clz {
			margin-left: 8px;
			border-bottom-left-radius: 10px;
			overflow: hidden;
			width: calc(100% - 8px - 8px) !important;
			border-top-left-radius: 10px;
			margin-top: 8px;
			border-top-right-radius: 10px;
			border-bottom-right-radius: 10px;
			margin-bottom: 8px;
			margin-right: 8px;
		}
		.image3-size {
			height: 80px !important;
			width: 100%;
		}
		.text2-clz {
			font-size: 16px !important;
		}
		.text3-clz {
			border: 1px solid #fb0909;
			padding-top: 3px;
			border-bottom-left-radius: 30px;
			color: #fb0909;
			padding-left: 15px;
			font-size: 16px !important;
			padding-bottom: 3px;
			border-top-right-radius: 30px;
			margin-right: 5px;
			margin-left: 5px;
			overflow: hidden;
			border-top-left-radius: 30px;
			margin-top: 5px;
			border-bottom-right-radius: 30px;
			margin-bottom: 5px;
			padding-right: 15px;
		}
		.flex5-clz {
			padding-top: 5px;
			padding-left: 5px;
			padding-bottom: 5px;
			padding-right: 5px;
		}
		.flex7-clz {
			margin-left: 5px;
			border-bottom-left-radius: 6px;
			box-shadow: 0px 1px 3px rgba(31, 31, 31, 0.16);
			overflow: hidden;
			width: calc(87.5% - 5px - 5px) !important;
			border-top-left-radius: 6px;
			margin-top: 5px;
			border-top-right-radius: 6px;
			border-bottom-right-radius: 6px;
			margin-bottom: 5px;
			margin-right: 5px;
		}
		.list1-item-clz {
			margin: 0px;
			padding: 10px;
		}
		.list1-item-clz .list1-icon-clz {
			width: 60px;
			height: 40px;
			font-size: calc(40px - 4px) !important;
		}
		.flex11-clz {
			margin-left: 5px;
			border-bottom-left-radius: 6px;
			box-shadow: 0px 1px 3px rgba(31, 31, 31, 0.16);
			overflow: hidden;
			width: calc(87.5% - 5px - 5px) !important;
			border-top-left-radius: 6px;
			margin-top: 5px;
			border-top-right-radius: 6px;
			border-bottom-right-radius: 6px;
			margin-bottom: 5px;
			margin-right: 5px;
		}
		.list6-item-clz {
			margin: 0px;
			padding: 10px;
		}
		.list6-item-clz .list6-icon-clz {
			width: 60px;
			height: 40px;
			font-size: calc(40px - 4px) !important;
		}
		.list4-item-clz {
			margin: 0px;
			padding: 10px;
		}
		.list4-item-clz .list4-icon-clz {
			width: 100px;
			height: 100px;
			font-size: calc(100px - 4px) !important;
		}
		.text5-clz {
			padding-top: 0px;
			padding-left: 10px;
			padding-bottom: 0px;
			padding-right: 10px;
		}
		.image-clz {
			margin-left: 8px;
			border-bottom-left-radius: 10px;
			overflow: hidden;
			flex: 1;
			border-top-left-radius: 10px;
			margin-top: 8px;
			border-top-right-radius: 10px;
			border-bottom-right-radius: 10px;
			margin-bottom: 8px;
			margin-right: 8px;
		}
		.image-size {
			height: 120px !important;
			width: 100% !important;
		}
		.text6-clz {
			padding-top: 0px;
			padding-left: 10px;
			padding-bottom: 0px;
			padding-right: 10px;
		}
		.image4-clz {
			margin-left: 8px;
			border-bottom-left-radius: 10px;
			overflow: hidden;
			flex: 1;
			border-top-left-radius: 10px;
			margin-top: 8px;
			border-top-right-radius: 10px;
			border-bottom-right-radius: 10px;
			margin-bottom: 8px;
			margin-right: 8px;
		}
		.image4-size {
			height: 80px !important;
			width: 100% !important;
		}
		.image6-clz {
			margin-left: 8px;
			border-bottom-left-radius: 10px;
			overflow: hidden;
			flex: 1;
			border-top-left-radius: 10px;
			margin-top: 8px;
			border-top-right-radius: 10px;
			border-bottom-right-radius: 10px;
			margin-bottom: 8px;
			margin-right: 8px;
		}
		.image6-size {
			height: 80px !important;
			width: 100% !important;
		}
		.container23496 {
			padding-left: 0px;
			padding-right: 0px;
		}
		.container23496 {
		}
	</style>
