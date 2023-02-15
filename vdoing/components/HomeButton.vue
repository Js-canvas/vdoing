<template>
    <router-link class="home-button" v-if="hasHomeLink" :to="getHomeData.homeBtnLink">
        <img :src="$withBase(getHomeData.homeBtnImg)"/>
    </router-link>
</template>

<script>
export default {
    data() {
        return { }
    },
    computed: {
        getHomeData() {
            return {
                ...this.$page.frontmatter
            }
        },
        hasHomeLink() {
            return !!this.getHomeData.homeBtnLink
        }
    },
}
</script>

<style lang="stylus" scoped>
    .home-button
        >img
            width 32px
            cursor pointer
</style>