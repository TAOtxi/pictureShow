<script>
import Picture from './components/Picture.vue'

export default {
    name: 'App',
    components: {
        Picture,
    },
    data() {
        return {
        }
    },
    methods: {
    },
    mounted() {
        setInterval(() => {
            let header = document.querySelector('#header h1');
            header.textContent = '番薯德，一个充满未知的人形生物';
            setTimeout(() => {
                header.textContent = '番薯德，一个充满未知的人形生物.';
            }, 300)
            setTimeout(() => {
                header.textContent = '番薯德，一个充满未知的人形生物..';
            }, 600)
            setTimeout(() => {
                header.textContent = '番薯德，一个充满未知的人形生物...';
            }, 900)
        }, 2000);
    }
}
</script>

<template>
    <div id="header" ref="header">
        <h1>番薯德，一个充满未知的人形生物</h1>
        <img class="head-img" src="./assets/fanshude.jpg">
        <img class="down" src="./assets/down.svg" @click="$refs.header.classList.toggle('hidden')">
    </div>
    <div class="container">
        <Picture prefix="demo" :picutre_counts="10"></Picture>
        <Picture prefix="1v1" :picutre_counts="7"></Picture>
        <Picture prefix="record" :picutre_counts="1"></Picture>
    </div>
</template>

<style scoped>
div#header {
    position: relative;
    height: 700px;
    background: linear-gradient(to right, rgb(15, 12, 41), rgb(48, 43, 99), rgb(36, 36, 62));
    color: white;
    transition: all 1.5s;
    overflow: hidden;
}

div#header.hidden {
    height: 90px;
}

h1 {
    transition: all 1.5s;
    padding: 20px;
    font-size: 80px;
    width: 100%;
    top: 50px
}

div#header.hidden h1 {
    transform: translateY(-100%);
}

div#header.hidden img.head-img {
    transform: translateY(-200%);
}

img.head-img {
    transition: all 1.5s;
    width: 400px;
    margin-top: 20px;
    margin-left: 20px;
    border-radius: 15px;
    outline: 5px solid gray;
}

img.down {
    width: 80px;
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    cursor: pointer;
}

div.container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    row-gap: 100px;
    margin-top: 50px;
}
</style>
