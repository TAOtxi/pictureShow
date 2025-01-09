<script>
export default {
    name: 'Header',
    data() {
        return {
        }
    },
    methods: {
        handleWheel(e) {
            if (e.deltaY > 0) {
                this.$refs.header.classList.add('hidden');
            }
            else {
                this.$refs.header.classList.remove('hidden');
            }
        }
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
    <div id="header" ref="header" @wheel.prevent="handleWheel">
        <h1>番薯德，一个充满未知的人形生物</h1>
        <img class="head-img" src="../assets/fanshude.jpg">
        <img class="down" src="../assets/down.svg" @click="$refs.header.classList.toggle('hidden')" draggable="false">
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
    transition: transform 1.5s;
    padding: 20px;
    font-size: max(40px, 4.5vw);
    width: 100%;
    top: 50px
}

div#header.hidden h1 {
    transform: translateY(-100%);
}


@keyframes header-rotate {
    0% {
        transform: rotate3d(0, 1, 0, 0deg);
    }
    50% {
        transform: rotate3d(0, 1, 0, 360deg);
    }
    100% {
        transform: rotate3d(0, 1, 0, 360deg);
    }
}

img.head-img {
    width: max(100px, 20vw);
    margin: 20px;
    border-radius: 15px;
    outline: 5px solid gray;
    animation: header-rotate 2.5s infinite;
}

@keyframes translateY {
    0% {
        transform: translateY(0);
    }
    100% {
        transform: translateY(-1000px);
    }
}

div#header.hidden img.head-img {
    animation: translateY 2.5s;
}

img.down {
    transition: all 1s;
    padding: 20px;
    width: 120px;
    height: 120px;
    position: absolute;
    bottom: 0px;
    left: 50%;
    transform: translateX(-50%);
    cursor: pointer;
    border-radius: 50%;
}

img.down:hover {
    box-shadow: inset 2px 2px 8px rgba(255, 255, 255, 0.1),
                inset -2px -2px 8px rgba(0, 0, 0, 0.2);
}

div#header.hidden img.down {
    bottom: -15px;
    box-shadow: none;
    transform: translateX(-50%) scaleY(-1);
}
</style>
