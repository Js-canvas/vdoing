<template>
	<form
		id="search-form"
		class="algolia-search-wrapper search-box"
		role="search"
	>
		<img
			v-if="isShowImg"
			class="search-blur"
			:src="$withBase('/img/search-blur.svg')"
			:style="{display: `${searchStatus === 'blur' ? 'block' : 'none'}`}"
		/>
		<img
			v-if="isShowImg"
			class="search-focus"
			:src="$withBase('/img/search-focus.svg')"
			:style="{display: `${searchStatus === 'focus' ? 'block' : 'none'}`}"
		/>
		<input
			id="algolia-search-input"
			:class="isAlgoliaNav ? 'search-query-nav' : 'search-query'"
			@focus="searchFocus"
			@blur="searchBlur"
			:placeholder="placeholder"
		/>
	</form>
</template>

<script>
export default {
	props: ['options', 'showImg', 'algoliaNav'],

	data () {
		return {
			placeholder: undefined,
			searchStatus: 'blur',
		}
	},
	computed: {
		isShowImg() {
			return typeof this.showImg === 'undefined' ? true : !!this.showImg
		},
		isAlgoliaNav() {
			return typeof this.algoliaNav === 'undefined' ? false : !!this.algoliaNav
		}
	},

	mounted () {
		this.initialize(this.options)
		this.placeholder = this.$site.themeConfig.searchPlaceholder || ''
	},

	methods: {
		initialize (userOptions) {
			Promise.all([
				import(/* webpackChunkName: "docsearch" */ 'docsearch.js/dist/cdn/docsearch.min.js'),
				import(/* webpackChunkName: "docsearch" */ 'docsearch.js/dist/cdn/docsearch.min.css')
			]).then(([docsearch]) => {
				docsearch = docsearch.default
				const { algoliaOptions = {} } = userOptions
				docsearch(Object.assign(
					{},
					userOptions,
					{
						inputSelector: '#algolia-search-input',
						// #697 Make docsearch work well at i18n mode.
						algoliaOptions: Object.assign({
							'facetFilters': ["*"].concat(algoliaOptions.facetFilters || [])
						}, algoliaOptions),
						handleSelected: (input, event, suggestion) => {
							const { pathname, hash } = new URL(suggestion.url, window.location.origin)
							const routepath = pathname.replace(this.$site.base, '/');
							const _hash = decodeURIComponent(hash)
							this.$router.push(`${routepath}${_hash}`)
						}
					}
				))
			})
		},

		update (options) {
			this.$el.innerHTML = '<input id="algolia-search-input" class="search-query">'
			this.initialize(options)
		},
		searchFocus () {
			this.searchStatus = 'focus'
		},
		searchBlur () {
			this.searchStatus = 'blur'
		}
	},

	watch: {
		options (newValue) {
			this.update(newValue)
		}
	}
}
</script>

<style lang="stylus">
.algolia-search-wrapper
	flex 1
	display flex
	.search-focus, .search-blur
		width 70px
		position absolute
		top -45px
		right 21px
		z-index 10
.algolia-autocomplete
	display flex!important
	flex 1
	.search-query, .search-query-nav
		flex 1
		background #fff
		border 1px solid #fff
		border-radius 50px
		padding-left 24px
	.search-query-nav
		border 1px solid #13b9e2
</style>
