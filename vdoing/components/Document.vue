<template>
	<div
		class="theme-container"
	>
		<Navbar v-if="shouldShowNavbar" :isSidebarOpen="isSidebarOpen" @toggle-sidebar="toggleSidebar"/>
		<Sidebar
			class="sidebar"
			:class="isSidebarOpen ? 'show-side' : 'unshow-side'"
			:items="sidebarItems"
			v-show="showSidebar"
		>
			<template #top v-if="sidebarSlotTop">
				<div
					v-html="sidebarSlotTop"
				></div>
			</template>
			<template #bottom v-if="sidebarSlotBottom">
				<div
					v-html="sidebarSlotBottom"
				></div>
			</template>
		</Sidebar>
		<Page :class="isSidebarOpen ? 'show-side-page' : 'full-page'">
			<template #top v-if="pageSlotTop">
				<div v-html="pageSlotTop"></div>
			</template>
			<template #bottom v-if="pageSlotBottom">
				<div v-html="pageSlotBottom"></div>
			</template>
		</Page>
	</div>
</template>

<script>
import Navbar from '@theme/components/Navbar.vue'
import Sidebar from '@theme/components/Sidebar.vue'
import Page from '@theme/components/Page.vue'
import { resolveSidebarItems } from '../util'

export default {
	components: { Navbar, Sidebar, Page },
	data() {
		return {
			isSidebarOpen: true,
			showSidebar: false,
		}
	},
	computed: {
		sidebarSlotTop() {
			return this.getHtmlStr('sidebarT')
		},
		sidebarSlotBottom() {
			return this.getHtmlStr('sidebarB')
		},
		pageSlotTop() {
			return this.getHtmlStr('pageT')
		},
		pageSlotBottom() {
			return this.getHtmlStr('pageB')
		},
		shouldShowNavbar() {
			const { themeConfig } = this.$site
			const { frontmatter } = this.$page
			if (
				frontmatter.navbar === false
				|| themeConfig.navbar === false) {
				return false
			}
			return (
				this.$title
				|| themeConfig.logo
				|| themeConfig.repo
				|| themeConfig.nav
				|| this.$themeLocaleConfig.nav
			)
		},
		shouldShowSidebar() {
			const { frontmatter } = this.$page
			return (
				!frontmatter.home
				&& frontmatter.sidebar !== false
				&& this.sidebarItems.length
				&& frontmatter.showSidebar !== false
			)
		},
		sidebarItems() {
			return resolveSidebarItems(
				this.$page,
				this.$page.regularPath,
				this.$site,
				this.$localePath
			)
		},
	},
	methods: {
		getHtmlStr(module) {
			const { htmlModules } = this.$themeConfig
			return htmlModules ? htmlModules[module] : ''
		},
		toggleSidebar() {
			this.isSidebarOpen = !this.isSidebarOpen
			this.$emit('toggle-sidebar', this.isSidebarOpen)
		},
	}
}
</script>

<style lang="stylus">
.sidebar
	display block !important
.unshow-side
	transform translateX(-100%)
.show-side-page
	.page
		padding-left 305px
		.theme-vdoing-wrapper
			margin 0
.full-page
	.theme-vdoing-wrapper
		margin 0 auto
</style>