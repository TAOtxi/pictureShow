<script>
export default {
    name: 'Picture',
    props: {
        prefix: {
            type: String,
            required: true
        },
        picutre_counts: {
            type: Number,
            required: true
        },
    },
    data() {
        return {
            source: [],
            mid: 0,
            hover: false,
        }
    },
    methods: {
        setSize(el, mid = False) {
            if (el.naturalHeight > el.naturalWidth) {
                el.style.height = mid ? '180%' : '120%';
            }
            else {
                el.style.width = mid ? '45%' : '30%';
            }
        },
        checkBorder(index) {
            if (index < 0)
                return this.source.length + index;

            if (index >= this.source.length)
                return index - this.source.length;

            return index;
        },
        clickImg(index) {
            if (index === this.mid)
                return;

            let leftIndex = this.checkBorder(this.mid - 1);
            let rightIndex = this.checkBorder(this.mid + 1);

            if (index !== leftIndex && index !== rightIndex) {
                return;
            }

            let direction = index === leftIndex ? 'left' : 'right';

            // Click on the right image
            if (direction === 'right') {
                this.$refs.container.children[this.mid].className = 'left top';

                let bottomIndex = this.checkBorder(this.mid - parseInt(this.source.length / 2));
                this.$refs.container.children[bottomIndex].className = 'right';

                this.$refs.container.children[this.checkBorder(leftIndex - 1)].classList.remove('second');
                this.$refs.container.children[leftIndex].classList.add('second');
                this.$refs.container.children[leftIndex].classList.remove('top');
                this.$refs.container.children[this.checkBorder(index + 1)].classList.add('top');
            }
            // Click on the left image
            else {
                this.$refs.container.children[this.mid].className = 'right top';

                let bottomIndex = this.checkBorder(this.mid - parseInt(this.source.length / 2) - 1);
                this.$refs.container.children[bottomIndex].className = 'left';

                this.$refs.container.children[this.checkBorder(rightIndex + 1)].classList.remove('second');
                this.$refs.container.children[rightIndex].classList.add('second');
                this.$refs.container.children[rightIndex].classList.remove('top');
                this.$refs.container.children[this.checkBorder(index - 1)].classList.add('top');
            }
            this.setSize(this.$refs.container.children[this.mid], false);
            this.$refs.container.children[index].className = 'mid';
            this.setSize(this.$refs.container.children[index], true);
            this.mid = index;
        },
        wheelImg(event) {
            let index = event.deltaY > 0 ? this.checkBorder(this.mid + 1) : this.checkBorder(this.mid - 1);
            this.clickImg(index);
        }
    },
    mounted() {
        for (let i = 0; i < this.picutre_counts; i++) {
            // if (fetch(`./src/assets/${i}.jpg`).status === 404) {
            //     continue;
            // }
            let imgURL = new URL(`../assets/${this.prefix}/${i}.jpg`, import.meta.url);
            this.source.push(imgURL.href);
        }
        if (this.source.length === 1) {
            this.source.push(this.source[0]);
            this.source.push(this.source[0]);
        }
        else if (this.source.length === 2) {
            this.source.push(this.source[0]);
            this.source.push(this.source[1]);
        }
        this.mid = parseInt(this.source.length / 2);

        this.$nextTick(() => {
            for (let i = 0; i < this.source.length; i++) {
                this.$refs.container.children[i].onload = () => this.setSize(this.$refs.container.children[i], i === this.mid);

                if (i < this.mid) {
                    this.$refs.container.children[i].className = 'left';
                }
                else if (i > this.mid) {
                    this.$refs.container.children[i].className = 'right';
                }
                else {
                    this.$refs.container.children[i].className = 'mid';
                }
            }
            this.$refs.container.children[this.mid - 1].classList.add('top');
            this.$refs.container.children[this.mid + 1].classList.add('top');

            setInterval(() => {
                if (!this.hover)
                    this.clickImg(this.checkBorder(this.mid + 1));
                this.hover = false;
            }, 3000);
        });
    }
}
</script>

<template>
    <div ref="container" @mouseover="hover = true" @mouseleave="hover = false">
        <img v-for="(src, index) in source" :src="src" :key="index" style="width: 30%;"
            @click="clickImg(index)" @wheel.prevent="wheelImg" />
    </div>
</template>

<style scoped>

div {
    position: relative;
    width: 1000px;
    height: 300px;
    /* outline: 3px solid gray; */
    border-radius: 15px;
    display: flex;
    justify-content: center;
    align-items: center;
}

img {
    outline: 3px solid gray;
    position: absolute;
    border-radius: 15px;
    transition: all 1s;
    z-index: 0;
}

img.top {
    z-index: 7;
}

img.second {
    z-index: 1;
}

img.mid {
    z-index: 15;
}

img.left {
    transform: translate(-300px, 0);
    filter: blur(2px);
}

img.right {
    transform: translate(300px, 0);
    filter: blur(2px);
}
</style>
