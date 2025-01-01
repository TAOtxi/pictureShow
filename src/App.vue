<script>
import Picture from './components/Picture.vue'

export default {
    name: 'App',
    components: {
        Picture,
    }
}
</script>

<template>
    <Picture prefix="demo" :picutre_counts="10" top="220"></Picture>
    <Picture prefix="1v1" :picutre_counts="7" top="580"></Picture>
    <Picture prefix="record" :picutre_counts="1" top="930"></Picture>
    <div class="expend"></div>
</template>

<style scoped>
div.expend {
    position: absolute;
    top: 1300px;
    width: 100%;
    height: 600px;
}

</style>
