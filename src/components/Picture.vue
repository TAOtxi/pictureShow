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
        width: {
            type: Number,
            default: 1000
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
        checkBorder(index) {
            if (index < 0)
                return this.source.length + index;

            if (index >= this.source.length)
                return index - this.source.length;

            return index;
        },
        EnlargeImg(index) {
            const background = document.createElement('div');
            const img = document.createElement('img');
            background.style = `
                position: fixed;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                background: rgba(0, 0, 0, 0.8);
                display: flex;
                justify-content: center;
                align-items: center;
                z-index: 100;
            `;
            img.src = this.source[index];

            img.style = `
                outline: 3px solid gray;
                border-radius: 15px;
                cursor: pointer;
                max-width: 60%;
                max-height: 90%;
            `;
            background.appendChild(img);
            document.body.appendChild(background);
            background.onclick = () => document.body.removeChild(background);
            background.onwheel = (event) => event.preventDefault();
        },
        clickImg(index) {
            if (index === this.mid) {
                this.EnlargeImg(index);
                return;
            }

            let leftIndex = this.checkBorder(this.mid - 1);
            let rightIndex = this.checkBorder(this.mid + 1);

            if (index !== leftIndex && index !== rightIndex) {
                return;
            }

            let bottomIndex;
            let direction = index === leftIndex ? -1 : 1;
            // Click on the right image
            if (direction === 1) {
                this.$refs.container.children[this.mid].className = 'left top';

                bottomIndex = this.checkBorder(this.mid - parseInt(this.source.length / 2));
                this.$refs.container.children[bottomIndex].className = 'right';

                this.$refs.container.children[this.checkBorder(leftIndex - 1)].classList.remove('second');
                this.$refs.container.children[leftIndex].classList.add('second');
                this.$refs.container.children[leftIndex].classList.remove('top');
                this.$refs.container.children[this.checkBorder(index + 1)].classList.add('top');
            }
            // Click on the left image
            else {
                this.$refs.container.children[this.mid].className = 'right top';

                bottomIndex = this.checkBorder(this.mid - parseInt(this.source.length / 2) - 1);
                this.$refs.container.children[bottomIndex].className = 'left';

                this.$refs.container.children[this.checkBorder(rightIndex + 1)].classList.remove('second');
                this.$refs.container.children[rightIndex].classList.add('second');
                this.$refs.container.children[rightIndex].classList.remove('top');
                this.$refs.container.children[this.checkBorder(index - 1)].classList.add('top');
            }
            this.$refs.container.children[index].className = 'mid';
            this.mid = index;
        },
        wheelImg(event) {
            let index = event.deltaY > 0 ? this.checkBorder(this.mid + 1) : this.checkBorder(this.mid - 1);
            this.clickImg(index);
        },
        handleMouseOver() {
            this.hover = true;
        }
    },
    mounted() {
        this.$refs.container.style.width = `min(${this.width}px, 90vw)`;
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
            }, 3000);
        });
    }
}
</script>

<template>
    <div ref="container" @mouseenter="hover = true" @mouseleave="hover = false">
        <img v-for="(src, index) in source" :src="src" :key="index" @click="clickImg(index)"
            @wheel.prevent="wheelImg" />
    </div>
</template>

<style scoped>
div {
    position: relative;
    aspect-ratio: 1000 / 300;
    /* outline: 3px solid gray; */
    border-radius: 15px;
    display: flex;
    justify-content: center;
    align-items: center;
}

img {
    outline: 0.2vw solid gray;
    position: absolute;
    border-radius: 1vw;
    transition: all 1s;
    z-index: 0;
    max-width: 30%;
    max-height: 120%;
}

img.top {
    z-index: 7;
}

img.second {
    z-index: 1;
}

img.mid {
    z-index: 15;
    cursor: zoom-in;
    transform: translateX(-50%) scale(1.5);
    left: 50%;
}

img.left {
    left: 20%;
    filter: blur(2px);
    translate: -50%;
}

img.right {
    left: 80%;
    transform: translateX(calc(50% - 100%));
    filter: blur(2px);
}
</style>
