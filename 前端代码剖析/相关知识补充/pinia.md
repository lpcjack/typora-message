# 一，pinia简介与优势

---

## 1，简介

- Pinia是vue生态里Vuex的替代者，==**一个全新的vue状态管理库。**==在Vue3成为正式版以后，尤雨溪强势推荐的项目就是Pinia。

---

##  2，优势

- 可以对Vue2和Vue3做到很好的支持，也就是老项目也可以使用Pinia。
- 抛弃了Mutations的操作，只有==**state、getters和actions.**==极大的简化了状态管理库的使用，让代码编写更加容易直观。
- 不需要嵌套模块，符合Vue3的Composition api ，让代码更加扁平化。
- ==**完整的TypeScript支持。**==Vue3版本的一大优势就是对TypeScript的支持，所以Pinia也做到了完整的支持。如果你对Vuex很熟悉的化，一定知道Vuex对TS的语法支持不是完整的（经常被吐槽）。
- 代码更加简洁，可以实现很好的代码自动分割。Vue2的时代，写代码需要来回翻滚屏幕屏幕找变量，非常的麻烦，Vue3的Composition api完美了解决这个问题。 可以实现代码自动分割，pinia也同样继承了这个优点。



---



# 二，pinia创建与安装

---

## 一，通过指令安装pinia，这一部分暂时省略！！！







---



## 二，用Pinia的方式创建一个store

### 1，在main.ts文件里引入Pinia

- ```typescript
  import { createPinia } from 'pinia'
  ```

  

- 引入后，通过createPinia( )方法，得到pinia的实例，然后将Pinia挂载到Vue根实例上。

  ![在这里插入图片描述](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401171128715.png)



---

### 2，创建store状态管理库

#### （1），引入

- `直接在/src目录下，新建一个store文件夹。有了文件夹之后，再创建一个index.ts文件。`
- 这个文件里的代码，我们一般只做三件事：
  1. `定义状态容器(仓库)`
  2. `修改容器(仓库)中的 state`
  3. `仓库中的 action 的使用`



#### （2），具体步骤以及添加内容

##### （Ⅰ），定义状态容器(仓库)

- ```typescript
  import { defineStore} from 'pinia'
  
  export const mainStore = defineStore('main',{
    state:()=>{
      return {}
    },
    getters:{},
    actions:{}
  })
  ```



- 以下是补充内容：

```
defineStore( ) 方法的第一个参数：相当于为容器起一个名字。注意：这里的名字必须唯一，不能重复。

defineStore( ) 方法的第二个参数：可以简单理解为一个配置对象，里边是对容器仓库的配置说明。当然这种说明是以对象的形式。

state 属性： 用来存储全局的状态的，这里边定义的，就可以是为SPA里全局的状态了。

getters属性： 用来监视或者说是计算状态的变化的，有缓存的功能。

actions属性： 对state里数据变化的业务逻辑，需求不同，编写逻辑不同。说白了就是修改state全局状态数据的。
```





##### （Ⅱ），我们在Store里定义一个State，我们这里就写Hello Pinia!。

- ```typescript
  state:()=>{
     return {
       helloPinia:'Hello Pinia!'
     }
  },
  ```

- 这时候这个helloPinia就是==全局的状态数据==，是每个页面和组件都==可以通过Pinia方法读取==到的。





##### （Ⅲ），在vue3组件里读取Store数据

- 在\src\components里，新建一个Hyy.vue的组件。代码如下：

  ```css
  
  <template>
    <div>
        <h2 class="">{{ store.helloPinia}}</h2>
    </div>
  </template>
  
  
  <script lang="ts">
  import { mainStore } from "../store/index";
  export default{
    setup(){
        const store = mainStore();
  
    return{
        store,
    }
  
    }
  }
  </script>
  ```

- 写好这个组件后，到App.vue里引入，就可以使用了：

  ```css
  <script setup lang="ts">
    import Hyy from "./components/Hyy.vue";
  </script>
  
  <template>
    <Hyy />
  </template>
  
  <style>
  </style>
  ```



---





# 三，Pinia改变状态数据和注意事项

---

## 1，Pinia改变状态数据

### （1），新建组件，实现状态数据的改变

- 为了演示数据仓库的概念，新建一个组件。然后在一个组件里修改状态数据，==看看另一个组件中的数据是否会改变==。

- **在\components\文件夹下新建一个文件CountButton.vue。**

  ```css
  <template>
      <h2 class="">{{ store.helloPinia }}</h2>
  </template>
  
  <script lang="ts">
  import { mainStore } from "../store/index";
  export default{
      setup(){
          const store = mainStore();
  
      return{
          store,
      }
  }
  
  }
  </script>
  ```

- **\src\store\index.ts文件**

  因为这里要做的是一个==可以计数的组件==，所以先到\store\index.ts的state属性中，==增加一个状态数据count : 0。==

  ```typescript
  state:()=>{
    return {
      helloWorld:'HelloWorld',
      count:0
    }
  },
  ```

- 有了这个状态数据后，再回到\components\\==CountButton.vue文件==里，增加**button和对应的业务逻辑**（注意这里的业务逻辑就是修改状态数据）。代码如下：

  ```css
  
  <template>
      <div>
          <button @click="handleClick">点击增加</button>
      </div>
  </template>
  
  <script lang="ts">
  import { mainStore } from "../store/index";
  //方法
  export default{
      setup(){
          const store = mainStore();
          const handleClick = () => {
              store.count ++
          }
  
      return{
          store,
          handleClick
      }
  }
  
  }
  </script>
  ```

- ***\src\components\Hyy.vue***

  写好后，我们把==count显示再Hyy.vue组件里。==

  ```css
  
  <template>
      <div>
          <h2>{{ store.helloPinia }}</h2>
          <h2>{{ store.count }}</h2>
      </div>
  </template>
  
  <script lang="ts">
  import { mainStore } from "../store/index";
  export default{
      setup(){
          const store = mainStore();
  
  
          return{
              store,
          }
      }
  
  }
  </script>
  ```

- **然后把CountButton加入到App.vue页面中。**

  ![image-20240117145148049](https://raw.githubusercontent.com/lpcjack/typora-message/main/assets/202401171451113.png)

  做完这步后，就可以到浏览器中查看一下最终的实现效果。如果一切正常，你可以看到我们点击按钮后，**两个组件的数据通过Pinia的状态管理**，已经可以实现联动了。

  

  

  

  

  



