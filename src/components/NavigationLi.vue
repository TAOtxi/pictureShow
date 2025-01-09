<script>
export default {
    name: 'NavigationLi',
    emits: ['nav-click'],
    props: {
        label: {
            type: String,
            required: true
        },
        list: {
            type: Array,
            required: true
        },
    }
}
</script>

<template>
    <li class="second-nav">
        <div>
            <span>{{ label }}</span>
            <img src="../assets/dropDown.svg" alt="arrow" />
        </div>
        <ul class="embed">
            <li v-for="(item, index) in list" :key="item" @click="$emit('nav-click', label, index)">{{ item }}</li>
        </ul>
    </li>
</template>

<style scoped>
li.second-nav {
    position: relative;
}

ul.embed {
    display: none;
    position: absolute;
    top: 43px;
    left: 0;
    overflow: hidden;
    list-style: none;
    background: white;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    width: 100%;
    font-size: 1em;
}

li.second-nav:hover ul.embed {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-size: 15px;
}

ul.embed li {
    padding: 10px 0;
    width: 100%;
    text-align: center;
}

ul.embed li:hover {
    background: #f0f0f0;
}

img {
    width: 15px;
    margin-left: 10px;
}


</style>
