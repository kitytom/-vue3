# 拥抱ts   TypeScript

打包大小减少41%

初次渲染快了55%

内存减少54%

源码升级


# 指令  第二种创建vue cli
vite创建  新一代 不依赖 webpack

优势 无需打包操作 可以快速冷启动
真正的按需加载  不在等待整个应用的编译完成

1 npm init vite-app vue3_test_vite
 npm i
npm dev

启动特别快

# main.js 入口文件
# 引入的是  createApp 工厂函数 无需通过new来调用 直接写名字就行
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

createApp(App).use(store).use(router).mount('#app')

#  mount('#app') 页面App.vue的id

# 常用的Composition API  组合式API

1拉开序幕的setup 新的配置项 值是一个函数

数据 、方法 、均要在setup 中 
  setup(){
   let name = 'zs'

  //  方法
  function asty(){
    alert(`wode${name}`)
  }
  // 交出去 才能使用
  return {
    name,
    asty
  }
  }

# 支持 data  和 methods
vue2 函数可以读取 vue3 配置的变量函数
vue3 函数不可以读取 vue2配置的函数和方法 

# vue3 和vue2 数据和方法不能混用  容易出错

# 冲突的时候尽 vue3 为主

# async 不能修饰 seteup 

# vue3 ref 函数   （vue2 ref是标签  给元素打标识 获取 相当于id）
ref可以继续使用 
vue3 多了一个 ref函数

# import {ref} from 'vue'
export default {
  name: 'App',
#  setup(){
  <!-- ref 引用对象 -->
    // 不是响应式数据 需要加上ref  是RefImpl一个对象
    //  Ref reference 引用  impl implemenr  引用的实现
    // value
#   let name =ref('zs') 
   let object=ref({
    type:'html',
    style:'vue'
   })

  //  方法
  function asty(){
    // alert(`wode${name}`)
    // name.value 这样才能拿到值
     name.value="李斯"
    // 更改对象
#    object.value.type='html5'
     object.value.style='vue3'
     console.log(name.value)
  }
#   // 交出去 才能使用
  return {
    name,
    asty,
    object
  }
  },
}

# reactive 响应式
 let nm=reactive({
    type:'html',
    style:'vue'
   })

  nm.style='ppp' 直接修改
# 可以直接输出对象 proxy 对象
vue3 可以监视到数据变化就可以实现响应式
Proxy {type: 'html', style: 'ppp'}

# reactive 可以处理数组

# sutup 注意点
 $atters

# 插槽
子组件 <slot>
 $solts 虚拟节点 存放父组件的传递的容器

  具名插槽 
  <template solt="tex1">
  <slot></solt>
  </template>



# computed 计算属性


 let funname = computed({
    // 读
    get(){
      return obj.age + obj.name
    },
    // 写
    set(){

    }
   })

#    // watch
// 监视多个值
  watch([name,obj],(newValue,olderVallue)=>{
    console.log('gaiiban'+newValue+olderVallue)
  })

# 监视 reactive 数组和对象


# // 直接监视对象名，是不能拿到 olderValue
  // 强制开启深度检测是无效的
#  watch(person,(newValue,olderVallue)=>{
    console.log(newValue,olderVallue)
  }),{deep:false}


 # // 可以监视其中的某个属性
  // 这样写成函数就可以拿到olderValue
  watch(()=>person.job.school,(newValue,olderVallue)=>{
    console.log(newValue,olderVallue)
  })


 #  // 监视对象中的属性对象 对象里面的属性是对象
  // 深度检测是无效的 无法检测到
  // watch(()=>person.job,(newValue,olderVallue)=>{
  //   console.log(newValue,olderVallue)
  // }),{deep:false}

 #  // 对象属性是对象需要.属性  拿到对象里面是对象的属性值才可以检测到
  watch(()=>person.job.school,(newValue,olderVallue)=>{
    console.log(newValue,olderVallue)
  }),{deep:false}


# 生命周期  最后一组 挂载 和卸载
# vue2
1、beforeCreate；
2、created；
3、beforeMount；
4、mounted；
5、beforeUpdate
；6、updated；
7、beforeDestroy；
8、destroyed。
# vue3
1.setup取代beforeCreate和created
vue3的组合式api中，setup中的函数执行相当于在选项api中的beforeCreate和created中执行

2.组合式api的生命周期需引入使用
除了beforeCreate和created外，其他生命周期的使用都需要提前引入（轻量化）

3.可使用的生命周期
除了beforeCreate和created被setup取代之外，选项式api和组合式api的映射如下：

# beforeMount -> onBeforeMount，在挂载前被调用

#  mounted -> onMounted，挂载完成后调用

# beforeUpdate -> onBeforeUpdate，数据更新时调用，发生在虚拟 DOM 打补丁之前。此时内存中的数据已经被修改，但还没有更新到页面上

# updated -> onUpdated，数据更新后调用，此时内存数据已经修改，页面数据也已经更新

# beforeUnmount -> onBeforeUnmount，组件卸载前调用

# unmounted -> onUnmounted，卸载组件实例后调用。

# errorCaptured -> onErrorCaptured，每当事件处理程序或生命周期钩子抛出错误时调用

# renderTracked -> onRenderTracked，状态跟踪，vue3新引入的钩子函数，只有在开发环境有用，用于跟踪所有响应式变量和方法，一旦页面有update，就会跟踪他们并返回一个event对象

# renderTriggered -> onRenderTriggered，状态触发，同样是vue3新引入的钩子函数，只有在开发环境有效，与onRenderTracked的效果类似，但不会跟踪所有的响应式变量方法，只会定点追踪发生改变的数据，同样返回一个event对象

# activated -> onActivated，与keep-alive一起使用，当keep-alive包裹的组件激活时调用

# deactivated -> onDeactivated，与keep-alive一起使用，当keep-alive包裹的组件停用时调用


 # //  渲染前 拿不到 demo
   onBeforeMount(() => {
    console.log('kk')
     
   }),
#     //  渲染 能拿到demo
    onMounted(()=>{
      console.log('渲染前')
    })
# 可以写两个相同生命周期钩子函数



vue生命周期判断一次 不在判断两次


vite工具的使用
下载项目
 1 npm init vite-app vue3dome
2 切换到目录下
cd 
npm install 下载包
npm run dev   启动项目



# toref 函数
用于将对象属性定义为响应式数据
 const obj =reactive( {
    name:'lizi'，
    adress：'fkjf'
 })

 const number = toRef(obj,'name')

更改值
number.value = '900'

使用场景
有一个响应式数据对象值用到其中一项就使用toRef
并且值是关联的

 const obj =reactive( {
    name:'lizi'，
    adress：'fkjf'
 })

# 如何直接在魔板中使用 属性名使用该属性
使用toref
使用结构赋值的数据不是响应式数据
{...}

cost boj2 = toRefs(obj)
这样结构出来的值就是响应式对象
先通过torefs  后再结构出来


# 父子通信
父组件传递给子组件
父传子
需要使用 props  子组件定义变量接收

父组件
<ChidPage :mon="name"></ChidPage>
mon 是子组件定义的变量
name 是父组件传递的值得变量
子组件
// 定义接受
  props:{
  mon:{
    type:toString,
    default:0
  }

# // 拿到传递的值
  setup(props){
    const name = ref('1')
    name.value=props.mon
   return {
      name
   }
  }


# 子传父
子组件传递给父组件

  setup(props,{emit}){
 const gtapp = ()=>{
      //  自定义事件
      emit('getput',num)
    }

    }

# 父组件
<ChidPage :mon="name" @getput="getch"></ChidPage>    
 let num= ref('')
    function getch(newnum){
     console.log(newnum)
     num.value=newnum
    }

    这样就可以拿到子组件传递的值


# 兄弟组件之间的传值  eventbus


# 父子通讯 可以使用 v-model
<ChidPage v-model:mon="name" @getput="getch"></ChidPage>  


# 依赖注入-------- 数据共享-----------
解决eventbus的繁琐
使用场景 一个大组件 有很多后台组价 
都想共享父组件的数据
# import { provide } from 'vue'
    // 将数据共享给后代  provide  后代可以直接使用
 provide('cant',name)

#  子组件
 import { inject, ref } from 'vue'
//  provide('cant',name)  父组件的值
// 子组件接收  inject 注入  使用定义的变量名就可以拿到
  const kant = inject('cant')
# 依赖注入 -----------修改数据--------- 单向的传递---只有父组件能更改
# 如何该数据
数据谁定义谁更改
通知父组件传值
 #   const change = () =>{
      console.log('00')
    }
    provide('change',change)


#  // provide('change',change) 使用父组件定义的方法
    // 是方法  使用change() 加上括号
    const change = inject('change')
可以使用方法来更改父组件的值 由父组件定义方法  子组件注入使用


# 补充 v-model 语法糖
:value   change 事件 改变的时候吧值给 value  就可以实现双向数据绑定

# vue3 v-model



