<template>
    <div>
        <ul>
            <li v-for="(item, index) in sList"
                :key="index"
                :class="{move: candelete.id === item.id}"
                @touchstart="touchStart(item)"
                @touchend="touchEnd(item, index)">
                {{item.text}}{{item.id}}
                <div class="del" @click="deleteItem(index)">删除</div>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                sList: [
                    {
                        id: 1,
                        text: '第一条数据'
                    },
                    {
                        id: 2,
                        text: '第二条数据'
                    },
                    {
                        id: 3,
                        text: '第三条数据'
                    },
                    {
                        id: 4,
                        text: '第四条数据'
                    },
                    {
                        id: 5,
                        text: '第五条数据'
                    },
                    {
                        id: 6,
                        text: '第六条数据'
                    },
                    {
                        id: 7,
                        text: '第七条数据'
                    }
                ],
                clientNum: {},  // 记录开始滑动（x1）,结束滑动（x2）的鼠标指针的位置
                candelete: {}   // 滑动的item
            }
        },
        methods: {
            // 删除item
            deleteItem(index) {
                this.sList.splice(index, 1)
                // splice方法是删除数组某条数据，或者向某个位置添加数据
            },
            touchStart() {
                event.preventDefault();
                let touchs = event.changedTouches[0];
                // 记录开始滑动的鼠标位置
                this.clientNum.x1 = touchs.pageX;
                this.candelete = {}
            },
            touchEnd(item, index) {
                if (event.target.innerHTML === '删除') {
                    this.deleteItem(index)
                } else {
                    let touchs = event.changedTouches[0];
                    // 记录结束滑动的鼠标位置
                    this.clientNum.x2 = touchs.pageX;
                    this.candelete = {}
                    // 判断滑动距离大于50，判定为滑动成功，否则失败
                    if (
                        this.clientNum.x2 < this.clientNum.x1 &&
                        Math.abs(this.clientNum.x1) - Math.abs(this.clientNum.x2) > 50
                    ) {
                        event.preventDefault();
                        this.candelete = item;
                    } else if (
                        this.clientNum.x2 > this.clientNum.x1 &&
                        Math.abs(this.clientNum.x2) - Math.abs(this.clientNum.x1) > 10
                    ) {
                        event.preventDefault();
                        this.candelete = {};
                    }
                }
            }
        }
    }
</script>

<style lang="scss" scoped>
ul{
    width: 100%;
    overflow-x: hidden; /*隐藏ul x轴的滚动条*/
    list-style: none;
    li{
        background: #fdfdfd;
        border-bottom: 1px solid #e1e1e1;
        line-height: 40px;
        position: relative;
        transform: translateX(0);
        transition: all .3s; /*滑动效果更生动*/
        padding-left: 10px;
        .del {
            position: absolute;
            top: 0;
            right: -1px;
            z-index: 3;
            width: 60px;
            height: 100%;
            text-align: center;
            color: #fff;
            background-color: #ff5b45;
            transform: translateX(60px); /*默认x轴位移60px，使其隐藏*/
        }
    }
    li.move {
        transform: translateX(-60px); /*滑动后x轴位移-60px,使其可见*/
    }
}
</style>