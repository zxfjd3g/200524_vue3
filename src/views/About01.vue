<template>
  <div class="about">
    <!-- 模板中操作ref响应式数据不用.value, 会自动添加 -->
    <h2>{{count}}</h2>
    <h2>{{double}}</h2>
    <h2>{{double2}}</h2>
    <button @click="update">更新</button>
    <button @click="double2 += 3">更新double2</button>
  </div>
</template>
<script>
  import {ref, computed} from 'vue'
  export default {
    name: 'About',
    // data () {
    //   return {
    //     count: 1
    //   }
    // },

    // methods: {
    //   update () {
    //     this.count++
    //   }
    // }

    beforeCreate() {
      console.log('beforeCreate()')
    },

    /* 
    初始化执行一次的配置回调: 在组件对象创建前执行, this是undefined
    所有的组合API都它里面执行
    */
    setup () {
      console.log('setup()', this)

      // 定义一个包含基本类型响应式数据的引用对象
      // 内部用value保存数据
      const count = ref(2)
      console.log(count)

      // 计算属性: getter
      const double = computed(() => {
        
        console.log('double getter')
        return count.value * 2
      })

      // 计算属性: getter & setter
      const double2 = computed({
        get () {
          console.log('double222 getter')
          return count.value * 2
        },
        set (value) {
          console.log('double22 setter')
          count.value = value/2
        }
      })


      // 更新响应式数据的函数
      function update () {
        count.value++
        // double2.value += 4
      }

      return {  // 返回的对象中的属性, 模板中可以直接操作
        count,
        double,
        double2,
        update,
        
      }
    }
  }
</script>