<template>
    <div class="test">
        <h1>vue3.0 初体验</h1>
        <p>少年你的头发可还好，？？？？ 哈哈哈哈哈</p>
        <h1>count:{{count}}</h1>
        <h1>double count {{doubleCount}}</h1>
        <div>state from vuex {{a}}</div>
        <button @click="add">add</button>
        <button @click="update">update a</button>
    </div>
</template>

<script>
import { ref, computed, watch, getCurrentInstance } from 'vue'
export default {
    //setup方法：创建组件实例，然后初始化 props ，紧接着就调用setup 函数。
    //从生命周期钩子的视角来看，它会在 beforeCreate 钩子之前被调用
    //setup 返回的 ref 在模板中会自动解开，不需要写 .value
    //也可以返回一个函数，函数中也能使用当前 setup 函数作用域中的响应式数据：
    setup() {
        const count = ref(0)
        //ref api用于创建可变更的普通的ref
        const add = () => {
            count.value++
        }
        const { ctx } = getCurrentInstance()
        console.log(ctx.$router.currentRoute.value)
        watch(
            () => count.value,
            val => {
                console.log(`count is ${val}`)
            }
        )
        //数据监听,第一个是监听数据，第二个可以执行监听回调
        // watch([refA, () => refB.value], ([a, b], [prevA, prevB]) => {
        //     console.log(`a is: ${a}`)
        //     console.log(`b is: ${b}`)
        // })·
        //两个以上的属性监听方式
        const doubleCount = computed(() => count.value * 2)
        const a = computed(() => ctx.$store.state.test.a)
        const update = () => {
            ctx.$store.commit('setTestA', count)
        }//update一次之后就发现相关数据和count变量绑定了
        return {
            count,
            doubleCount,
            add,
            a,
            update
        }
    }
}
</script>

<style lang="less" scoped>
.test {
    color: red;
}
</style>