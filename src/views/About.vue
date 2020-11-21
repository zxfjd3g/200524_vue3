<template>
  <div class="about">
    <!-- 模板中操作ref响应式数据不用.value, 会自动添加 -->
    <h2>count: {{count}}</h2>
    <h2>msg: {{msg}}</h2>
    <h2>user: {{user}}</h2>
    <h2>numbers: {{numbers}}</h2>
    <button @click="update">更新</button>
  </div>
</template>
<script>
  import {reactive, toRefs, watch, ref, warn, watchEffect} from 'vue'
  export default {
    name: 'About',

    /* 
    初始化执行一次的配置回调: 在组件对象创建前执行, this是undefined
    所有的组合API都它里面执行
    */
    setup () {
      console.log('setup()', this)

      /* 对多个数据实现响应式处理 */
      /* 
      包含响应式数据的代理对象
      只有通过代理对象操作它内部的数据才是响应式
      */
      const state = reactive({
        msg: 'abc',
        user: {
          // name: 'tom'
          age: 1
        },
        numbers: [1, 3, 5]
      })

      const count = ref(0)

      // toRefs: 将state中每个属性用ref包装成响应式对象, 返回包含这些响应式对象的一般对象
      // const stateRefs = toRefs(state)
      // console.log(stateRefs)

      // console.log(state, state.user)


      // 监视ref
      watch(count, (value) => {
        console.log('count变化了', value, count.value)
      }, {
        immediate: true
      })
      // 监视reactive中的某个属性
      watch(() => state.msg, (value) => {
        console.log('msg变化了', value, state.msg)
      })

      // 监视多个数据
      watch([count, () => state.msg], (values) => {
        console.log('count或msg变化了', values)
      })

      // 监视所有用到的响应式数据
      watchEffect(() => {
        console.log('watchEffect()')
        console.log(count.value)
      })




      const update = () => {
        state.msg += '--'
        // state.user.name += '++'
        // delete state.user.age
        // state.numbers.push(7)
        // state.numbers[0] = 0
        count.value++
      }
      /* 
      Vue2的问题
        对象: 直接添加新属性不是响应式 / 直接删除属性界面不会自动更新
        数组: 通过下标直接修改数组内数据界面不会自动更新
      Vue3
        都可以
      */


      return {
        // state,
        // ...state,  // error
        // ...stateRefs,
        ...toRefs(state),
        count,

        update
      }
    }
  }
</script>