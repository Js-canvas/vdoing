<template>
	<aside class="sidebar">
		<SidebarLinks :depth="0" :items="items" />
	</aside>
</template>

<script>
import SidebarLinks from '@theme/components/SidebarLinks.vue'

export default {
	name: 'Sidebar',

	components: { SidebarLinks },

	props: ['items'],

	computed: {
	}
}
</script>

<style lang="stylus">
.sidebar
	ul
		padding 0
		margin 0
		list-style-type none
	a
		display inline-block
	.nav-links
		display none
		border-bottom 1px solid var(--borderColor)
		padding 0.5rem 0 0.75rem 0
		a
			font-weight 600
		.nav-item, .repo-link
			display block
			line-height 1.25rem
			font-size 1.1em
			padding 0.5rem 0 0.5rem 1.5rem
	& > .sidebar-links
		padding 1.5rem 0
		& > li > a.sidebar-link
			font-size 1.1em
			line-height 1.7
			font-weight bold
		& > li:not(:first-child)
			margin-top 0.75rem
</style>
