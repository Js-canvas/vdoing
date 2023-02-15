<template>
    <div class="container-bg">
        <div
            v-for="(item, index) in getImgList"
            :key="index"
            class="bg-item"
            :style="{backgroundImage: item.url}"
        ></div>
        <div class="layout-show-info">
            <!-- <span class="text-flicker-in-glow">前端知识</span> -->
            <span class="text-flicker-in-glow-other">学无止境</span>
        </div>
        <div class="algo-wrap">
            <AlgoliaSearchBox
                class="algolia-search"
                v-if="isAlgoliaSearch"
                :options="algolia"
            />
        </div>
        <div class="layout-show-btn">
            <HomeButton class="button-shake-bottom" />
        </div>
    </div>
</template>
<script>
import AlgoliaSearchBox from '@theme/components/AlgoliaSearchBox'
import HomeButton from '@theme/components/HomeButton'
export default {
    components: { AlgoliaSearchBox, HomeButton },
    data() {
        return { }
    },
    computed: {
        getHomeData() {
            return {
                ...this.$page.frontmatter
            }
        },
        getImgList() {
            const { bannerList = []} = this.getHomeData

            return bannerList.map(item => ({
                url: `url(${this.$withBase(item.url)})`
            }))
        },
        algolia () {
            return this.$themeLocaleConfig.algolia || this.$site.themeConfig.algolia || {}
        },

        isAlgoliaSearch () {
            return this.algolia && this.algolia.apiKey && this.algolia.indexName
        }
    },
    created() {
    },
    beforeMount() {
    },
    mounted() {

    },
    watch: {
    },
    methods: {
    }
}
</script>
<style lang="stylus" scoped>
    @require('../styles/text-flicker.styl')
    @require('../styles/button-vibrate.styl')

    .container-bg
        width 100%
        height 100%
        position fixed
        top 0
        bottom 0
        left 0
        right 0
        overflow hidden
        .algo-wrap
            width 350px
            height 40px
            position absolute
            top calc(50% - 20px)
            left calc(50% - 175px)
            z-index 999
            display flex
        .layout-show-info
            top calc(50% - 160px)
        .layout-show-info, .layout-show-btn
            width 350px
            position absolute
            left calc(50% - 175px)
            z-index 999
            display flex
            align-items center
            justify-content center
            flex-direction column
            >span
                font-size 46px
                color #fff
            .text-flicker-in-glow-other
                animation text-flicker-in-glow 6s linear both
            .button-shake-bottom:hover
                animation shake-bottom .8s cubic-bezier(0.455, 0.030, 0.515, 0.955) both
        .layout-show-btn
            width auto
            left calc(50% + 132px)
            top calc(50% - 16px)
        .bg-item
            width 100%
            height 100%
            position absolute
            background no-repeat 50% 50%
            background-size cover
            animation-duration 50s
            animation-timing-function linear
            animation-iteration-count infinite
            opacity 1
            transform scale(1.2)
            &:first-child
                animation-name change-1
                z-index 6
            @keyframes change-1
                0
                    opacity 1
                    transform scale(1.2)
                15%
                    opacity 1
                20%
                    opacity 1
                25%
                    opacity 0
                    transform scale(1)
                100%
                    opacity 0
                    transform scale(1.2)
                98%
                    opacity 0
                    transform scale(1.2)
                100%
                    opacity 1
            &:nth-child(2)
                animation-name change-2
                z-index 5
            @keyframes change-2
                20%
                    opacity 1
                    transform scale(1.2)
                28%
                    opacity 1
                40%
                    opacity 1
                45%
                    opacity 0
                    transform scale(1)
                100%
                    opacity 0
                    transform scale(1.2)
            &:nth-child(3)
                animation-name change-3
                z-index 4
            @keyframes change-3
                40%
                    opacity 1
                    transform scale(1.2)
                51%
                    opacity 1
                60%
                    opacity 1
                65%
                    opacity 0
                    transform scale(1)
                100%
                    opacity 0
                    transform scale(1.2)
                98%
                    opacity 0
                    transform scale(1.2)
                100%
                    opacity 1
            &:nth-child(4)
                animation-name change-4
                z-index 3
            @keyframes change-4
                60%
                    opacity 1
                    transform scale(1.2)
                76%
                    opacity 1
                80%
                    opacity 1
                85%
                    opacity 0
                    transform scale(1)
                100%
                    opacity 0
                    transform scale(1.2)
            &:nth-child(5)
                animation-name change-5
                z-index 2
            @keyframes change-5
                80%
                    opacity 1
                    transform scale(1.2)
                87%
                    opacity 1
                93%
                    opacity 1
                98%
                    opacity 1
                100%
                    opacity 0
                    transform scale(1)

</style>
