### vuex

#### state

提供唯一的公共数据源，所有共享的数据都要统一放到Store的state中进行存储

```vue
const store = new Vuex.Store({
	state: { const: 0 }
})
```

state是store.js 的一个属性

##### 1.组件中访问state中的数据的第一种方法

`this.$store.state.全局数据名称`

##### 2.组件中访问state中的数据的第二种方法
 * 从vuex中按需导入mapState函数

     import { mapState } from 'vuex'

* 通过刚才导入的mapState函数，将当前组件需要的全局数据映射为当前组件的computed计算属性

```js
computed: {
    ...mapState(['count'])
}
```

#### mutation

用于修改store中的数据

* 只能通过mutation变更store中的数据，不可以直接操作store中的数据
* 通过mutation的方式操作起来虽然繁琐，但是可以集中监控所有的数据变化

##### 1.触发mutation的第一种方法

```js
//定义mutation
const store = new Vuex.Store({
  state: {
    count: 0 
  },
  mutations: {
    add(state) {
      state.count++
    },
    //加N操作
    addN(state,step) {
      state.count+= step
    }
  },
  actions: {
  },
  modules: {
  }
})
```

```js
//触发mutation
export default {
    data() {
        return{}
    },
    methods: {
        btnHandler1(){
            this.$store.commit('add')
        }
    }
}

```

##### 2.触发mutation的第二种方法

* 从vuex中导入mapMutations函数

  import { mapMutations } from 'vuex'

* 通过刚才导入的mapMutations函数，将需要的mutations函数，映射到当前组件的methods方法：

```js
methods: {
    ...mapMutations(['add', 'addN'])
}
```

#### action

action用于处理异步任务

```js
//定义Action
const store = new Vuex.Store({
    //...省略其他代码
    mutations： {
    	add(state) {
    		state.count++
		}
	},
    actions: {
        //在action中，不能直接修改state中的数据；
        //必须通过context.commit()触发某个mustation
        addAsync(context) {
            setTimeout(() => {
                context.commit('add')
            }, 1000)
        }
    }
})
```

##### 1.触发action的第一种方式

```js
//触发action
methods: {
    handle() {
        //触发action的第一种方式
        this.$store.dispatch('addAsync')
    }
}
```

##### 2.触发action的第二种方式

* 从vuex中导入mapActions函数

`import { mapActions } from  vuex`

* 通过刚才导入的mapActions函数，将需要的actions函数映射为methods

```js
...mapActions(['subAsync', 'subNAsync'])
```

#### getter

##### 1.触发getter的第一种方式

`this.$store.getters.名称`

##### 2.触发getter的第二种方式

* 从vuex中导入mapGetters函数

`import { mapGetters } from vuex`

* 通过刚才导入的mapGetters函数，将需要的getters函数映射为computed属性

`......mapGetters(['showNum'])`

