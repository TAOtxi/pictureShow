<script>
import NavigationLi from './NavigationLi.vue';
export default {
    name: 'Navigation',
    components: {
        NavigationLi
    }
}
</script>

<template>
    <nav>
        <p>𝑬𝒕𝒆𝒓𝒏𝒊𝒕𝒚</p>
        <ul>
            <NavigationLi label="番薯德" :list="['单挑败绩', '诗人比赛', '赤石瞬间']" @nav-click="(label, index) => $emit('nav-click', label, index)"/>
            <NavigationLi label="作品" :list="['画作', '音乐', '视频']" @nav-click="(label, index) => $emit('nav-click', label, index)"/>
            <li>作品</li>
            <li>关于</li>
        </ul>
    </nav>
</template>

<style scoped>
nav {
    position: sticky;
    top: 0;
    z-index: 50;
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 100%;
    height: 60px;
    background: white;
}

p {
    color: #31B4EA;
    text-shadow:
        1px 0 0 rgba(255, 255, 255, 0.2),
        -1px 0 0 rgba(255, 255, 255, 0.2),
        0 1px 0 rgba(255, 255, 255, 0.2),
        0 -1px 0 rgba(255, 255, 255, 0.2),
        1px 1px 0 rgba(255, 255, 255, 0.2),
        -1px -1px 0 rgba(255, 255, 255, 0.2),
        1px -1px 0 rgba(255, 255, 255, 0.2),
        -1px 1px 0 rgba(255, 255, 255, 0.2);
    flex: 1;
    text-align: center;
    font-size: 50px;
}

ul {
    flex: 6;
    display: flex;
    justify-content: flex-start;
    gap: 50px;
    list-style: none;
    font-size: 18px;
    margin-left: 60px;
}

li {
    cursor: pointer;
    border-radius: 10px;
    padding: 10px;
}

li:hover {
    background-color: #e7eaec;
}

</style>