<template>
<div>
    <button @click="handleClick">click</button>
    <button @click="stopwatch">stop watch</button>
    <button @click="startWatch">startWatch</button>
    <div>{{count}}</div>
    <div>time: {{state.time}}</div>
    <div>dou: {{dou}}</div>
</div>
</template>

<script>
import {
    onMounted,
    defineComponent,
    ref,
    reactive,
    computed,
    watchEffect,
    watch
} from "vue";

// mounted可以抽出
function test() {
    onMounted(() => {});
}

export default defineComponent({
    name: "HelloWorld",
    props: {
        msg: String
    },
    setup() {
        const count = ref(0); // 针对数值型,RefImpl
        const state = reactive({
            time: 0
        }); // 针对引用类型 proxy
        console.log("count: ", count);

        console.log("state: ", state);
        const handleClick = () => {
            count.value++;
            state.time++;
        };

        const dou = computed(() => {
            return count.value * 2;
        });
        console.log("dou: ", dou); // ComputedRefImpl

        // watchEffect 不用指定监听那个变量，但是无法获取旧值
        const stop = watchEffect(() => {
            console.log("dou New", state.time);
        });
        const stop2 = watch(() => {
            console.log("dou New", state.time);
        });

        // 主动停止监听
        const stopwatch = () => {
            stop();
            stop2();
        };

        const startWatch = () => {
            watchEffect(() => {
                console.log("dou New", state.time);
            });
        };

        return {
            dou,
            count,
            state,
            stopwatch,
            startWatch,
            handleClick
        };
    }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
</style>
