<script>
import Header from './components/Header.vue'
import Picture from './components/Picture.vue'

export default {
    name: 'App',
    components: {
        Header,
        Picture,
    },
    data() {
        return {
        }
    },
}
</script>

<template>
    <Header></Header>
    <div class="container">
        <Picture prefix="demo" :picutre_counts="10"></Picture>
        <Picture prefix="1v1" :picutre_counts="7"></Picture>
        <Picture prefix="record" :picutre_counts="1"></Picture>
    </div>
</template>

<style scoped>
div.container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    row-gap: 100px;
    margin-top: 50px;
}
</style>
