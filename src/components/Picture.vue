<script>
export default {
    name: 'Picture',
    data() {
        return {
            source: [
                'src/assets/pic.jpg',
                'src/assets/pic1.jpg',
                'src/assets/pic2.jpg',
                'src/assets/pic3.jpg',
                'src/assets/pic4.jpg',
                'src/assets/pic5.jpg',
                'src/assets/pic6.jpg',
                'src/assets/pic7.jpg',
                'src/assets/pic8.jpg',
                'src/assets/pic9.jpg',
            ],
            mid: 0,
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

                this.$refs.container.children[leftIndex].classList.remove('top');
                this.$refs.container.children[this.checkBorder(index + 1)].classList.add('top');
            }
            // Click on the left image
            else {
                this.$refs.container.children[this.mid].className = 'right top';

                let bottomIndex = this.checkBorder(this.mid - parseInt(this.source.length / 2) - 1);
                this.$refs.container.children[bottomIndex].className = 'left';

                this.$refs.container.children[rightIndex].classList.remove('top');
                this.$refs.container.children[this.checkBorder(index - 1)].classList.add('top');
            }
            this.setSize(this.$refs.container.children[this.mid], false);
            this.$refs.container.children[index].className = 'mid';
            this.setSize(this.$refs.container.children[index], true);
            this.mid = index;

            console.log('-------------------');
            for (let img of this.$refs.container.children) {
                console.log(img.className);
            }
        },
        wheelImg(event) {
            let index = event.deltaY > 0 ? this.checkBorder(this.mid + 1) : this.checkBorder(this.mid - 1);
            this.clickImg(index);
        }

    },
    mounted() {
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
                this.setSize(this.$refs.container.children[i], i === this.mid);

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
        });
    }
}
</script>

<template>
    <p>鼠标滚轮滑动或点击两侧图片轮播</p>
    <div ref="container">
        <img v-for="(src, index) in source" :src="src" :key="index" @click="clickImg(index)"
            @wheel="wheelImg" />
    </div>
</template>

<style scoped>
p {
    text-align: center;
    font-size: 20px;
    margin-top: 100px;
}

div {
    position: absolute;
    width: 50%;
    height: 30%;
    left: 50%;
    top: 35%;
    transform: translate(-50%, -50%);
    outline: 3px solid gray;
    border-radius: 15px;
    display: flex;
    justify-content: center;
    align-items: center;
}

img {
    outline: 3px solid gray;
    position: absolute;
    border-radius: 15px;
    transition: transform 1s, filter 1s, width 1s, height 1s;
    z-index: 0;
}

img.top {
    z-index: 1;
}

img.mid {
    z-index: 5
}

img.left {
    transform: translate(-15vw, 0);
    filter: blur(2px);
}

img.right {
    transform: translate(15vw, 0);
    filter: blur(2px);
}
</style>
