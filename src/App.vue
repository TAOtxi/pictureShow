<script>
import Header from './components/Header.vue'
import Picture from './components/Picture.vue'
import Navigation from './components/Navigation.vue'

export default {
    name: 'App',
    components: {
        Header,
        Picture,
        Navigation
    },
    data() {
        return {
        }
    },
    methods: {
        handleNavClick(label, index) {
            console.log(label, index);
        }
    }
}
</script>

<template>
    <Header></Header>
    <Navigation @nav-click="handleNavClick"></Navigation>
    <div class="container">
        <Picture prefix="demo" :picutre_counts="10" :width="1500"></Picture>
        <Picture prefix="1v1" :picutre_counts="7" :width="1500"></Picture>
        <Picture prefix="record" :picutre_counts="1" :width="1500"></Picture>
    </div>
</template>

<style scoped>
div.container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    row-gap: 100px;
    margin-top: 150px;
}
</style>
