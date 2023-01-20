<template>
    <div class="container-bg">
        <Home v-if="isHome" />
        <Document v-else-if="isDocument" />
    </div>
</template>
<script>
import Home from '@theme/components/Home'
import Document from '@theme/components/Document'
export default {
    components: { Home, Document },
    computed: {
        isHome() {
            return this.$route.fullPath === '/'
        },
        isDocument() {
            return this.$route.fullPath !== '/'
        },
    },
}
</script>
<style scoped>
    .container-bg {
        font-family: zkgd !important;
    }
</style>
