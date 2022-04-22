<template>
	<form
		id="search-form"
		class="algolia-search-wrapper search-box"
		role="search"
	>
		<img
			class="search-blur"
			src="/img/search-blur.svg"
			:style="{display: `${searchStatus === 'blur' ? 'block' : 'none'}`}"
		/>
		<img
			class="search-focus"
			src="/img/search-focus.svg"
			:style="{display: `${searchStatus === 'focus' ? 'block' : 'none'}`}"
		/>
		<input
			id="algolia-search-input"
			class="search-query"
			@focus="searchFocus"
			@blur="searchBlur"
			:placeholder="placeholder"
		/>
	</form>
</template>

<script>
export default {
	props: ['options'],

	data () {
		return {
			placeholder: undefined,
			searchStatus: 'blur',
		}
	},

	mounted () {
		this.initialize(this.options, this.$lang)
		this.placeholder = this.$site.themeConfig.searchPlaceholder || ''
	},

	methods: {
		initialize (userOptions, lang) {
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
							const { pathname, hash } = new URL(suggestion.url)
							const routepath = pathname.replace(this.$site.base, '/');
							const _hash = decodeURIComponent(hash)
							this.$router.push(`${routepath}${_hash}`)
						}
					}
				))
			})
		},

		update (options, lang) {
			this.$el.innerHTML = '<input id="algolia-search-input" class="search-query">'
			this.initialize(options, lang)
		},
		searchFocus () {
			console.log('focus')
			this.searchStatus = 'focus'
		},
		searchBlur () {
			console.log('blur')
			this.searchStatus = 'blur'
		}
	},

	watch: {
		$lang (newValue) {
			this.update(this.options, newValue)
		},

		options (newValue) {
			this.update(newValue, this.$lang)
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
	// .search-blur
	// 	display block
	// .search-focus
	// 	display none
	// .search-query:focus
	// 	background #f00
	// 	.serach-focus
	// 		display block
	// 	.search-blur
	// 		display none
.algolia-autocomplete
	display flex!important
	flex 1
	.search-query
		flex 1
		background #fff
		border 1px solid #fff
		border-radius 50px
		padding-left 24px
</style>
