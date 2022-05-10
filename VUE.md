#### node.js 使用ES6模块化

![63741923969](VUE.assets/1637419239695.png)

npm  init -y 快速初始化创建package.json   添加 “type‘:"module"



#### 默认导出语法  export default(默认导出的成员)

![63741975265](VUE.assets/1637419752651.png)

 

#### 默认导入 import xx from xxxx

![63742005332](VUE.assets/1637420053326.png)

注：只能做一次默认导出  export default

![63742038072](VUE.assets/1637420380724.png)

导入时，变量名随意，合法即可



#### 按需导出/导入

![63742098671](VUE.assets/1637420986715.png)

#### 按需导入  使用{}，变量名必须和导出时一致，可使用as重命名

![63742102391](VUE.assets/1637421023917.png)

![63742118418](VUE.assets/1637421184188.png)

![63742158423](VUE.assets/1637421584232.png)



#### 直接导入并执行模块中的代码

![63742176998](VUE.assets/1637421769980.png)





## Promise

#### 回调地狱

![63742200950](VUE.assets/1637422009505.png)

![63748582899](VUE.assets/1637485828994.png)

#### promise解决fs.readFile()回调地狱

需要按顺序读取文件，readFile()是异步读取 直接多次调用无法保证读取顺序，需要嵌套在readFile（）方法的回调函数里，变成"回调地狱"(多个回调嵌套)

![63748736943](VUE.assets/1637487369438.png)

![63748770497](VUE.assets/1637487704975.png)

![63748830234](VUE.assets/1637488302346.png)

#### .catch(reject())捕捉错误

如果前面的then()，传递了错误回调的方法，就不会执行.catch()方法里的回调函数

前面then里没传，才会执行.catch里的错误回调

相关源码，其实.catch调用的还是then，传入回调函数

是否执行reject，then方法里做了相关的判断

![65140578337](VUE.assets/1651405783376.png)

![63748889535](VUE.assets/1637488895359.png)

#### promise.all() 

![63749080138](VUE.assets/1637490801383.png)



#### Promise.race()

 ![63749109399](VUE.assets/1637491093995.png)

####  自己封装getFile()方法，通过Promise实现

![63749863437](VUE.assets/1637498634378.png)



### 下图是错误解释

![63749619777](VUE.assets/1637496197779.png)

上图是**错误**的，resolve和reject是promise类里自定义的变量(或者标记),

new Promise时，内部定义的方法就已经执行。正常操作是在定义内部需要执行的方法时根据执行成功或者失败，设置调用resolve()或reject()方法，**而resolve()和reject()方法是Promise对象自身封装的方法**，调用resolve()或reject()方法操作，是把执行成功or失败以及获取到的数据(调用resolve时作为形参传入)保存在Promise自身定义的变量里。

外部调用then方法时，就会**根据对象里保存的成功或失败结果**，执行用户给then方法传入的成功或失败回调函数

同时也将需要传递的数据传入回调方法中。



所以使用Promise时，需要在.Promise()后.then，.Promise().then()，在then里定义成功回调函数，才可以拿到结果值(赋值或者return)

因为then方法是Promise对象里的方法，写代码时不调用then或者不传递成功回调，就不会对之前resolve()方法保存在Promise对象里的结果数据做任何操作，也就无法拿到结果值。



如果是**await方式**，await相当于then的语法糖，把await后的所有操作都包裹为成功回调resolve()，然后传递给then



![63749643325](VUE.assets/1637496433257.png)

#### async/await  (ES7出现)

#### 重：async/await相当于promise里then操作的语法糖

const result=await Promise(xxx) 

console.log(result)

**await相当于把await之后的操作包裹为一个成功回调resolve(val)，然后传入then方法里执行，**

即resolve(val){const result=val;console.log(result)}, Promise.then(resolve())

所以说await后的所有代码，相对于async外面的代码是异步的，因为await后的所有代码都是then的成功回调函数resolve()的内容

且，await Promise，该Promise如果执行失败，await后的所有代码也不会执行，因为await后的代码相当于成功回调方法里的内容

![63749781407](VUE.assets/1637497814074.png)

![63749905580](VUE.assets/1637499055801.png)

注意事项

![63749977030](VUE.assets/1637499770300.png)



#### EventLoop

![63750007576](VUE.assets/1637500075767.png)

![63750026085](VUE.assets/1637500260854.png)

#### 宏任务和微任务

![63750150954](VUE.assets/1637501509547.png)

![63750228994](VUE.assets/1637502289947.png)

注：new Promise(function(){}) 是同步任务，会立即执行function函数的内容



## Webpack

#### 初始化

![63750955818](VUE.assets/1637509558187.png)

#### 打包

![63758584183](VUE.assets/1637585841833.png)

#### 修改入口出口

![63758645738](VUE.assets/1637586457381.png)

![63758647917](VUE.assets/1637586479178.png)

![63758671681](VUE.assets/1637586716818.png)

#### HtmlWebpackPlugin 插件 自动生成入口html文件，并引入打包好的js文件

![63758792574](VUE.assets/1637587925741.png)

![63758795580](VUE.assets/1637587955802.png)

#### 打包css

css&style加载器

在打包入口的.js文件中import css

![63758861412](VUE.assets/1637588614120.png)

直接import的css文件无法识别 需要安装css loader style loader

![63758873060](VUE.assets/1637588730603.png)

![63758877519](VUE.assets/1637588775197.png)

#### less-loader

![63758897322](VUE.assets/1637588973223.png)

#### assert module资源模块  webpack5自带 打包静态资源文件

![63759255219](VUE.assets/1637592552199.png)

![63759259410](VUE.assets/1637592594106.png)

打包fonts文件

![63759343202](VUE.assets/1637593432020.png)



#### babel  babel-loader  (实现对js语法降级解决兼容问题)

![63767214330](VUE.assets/1637672143300.png)



webpack开发服务器

![63767323071](VUE.assets/1637673230718.png)

![63767356995](VUE.assets/1637673569959.png)

端口

![63767366677](VUE.assets/1637673666777.png)



## Vue

#### vue/cli 脚手架创建项目

npm install  @vue/cli -g  安装vue/cli

![63767874744](VUE.assets/1637678747440.png)

npm run serve开启热更新服务器

![63768069397](VUE.assets/1637680693972.png)



#### @vue/cli配置端口号

vue.config.js

![63768079899](VUE.assets/1637680798990.png)



#### eslint代码检查工具

![63768124674](VUE.assets/1637681246744.png)

![63768132900](VUE.assets/1637681329009.png)



#### 单Vue文件开发方式

![63776189931](VUE.assets/1637761899316.png)



#### Vue  插值表达式

![63776454052](VUE.assets/1637764540527.png)

MVVM设计模式

![63776495305](VUE.assets/1637764953053.png)



#### V-bind

![63776538480](VUE.assets/1637765384808.png)

#### v-on绑定事件

![63776571412](VUE.assets/1637765714128.png)

#### this指向当前export的对象

![63776583309](VUE.assets/1637765833098.png)

#### @事件名=“函数”

![63776586399](VUE.assets/1637765863996.png)

#### **native后缀(加在@事件名后面)**

**自定义组件**不加.native不会执行原生事件(如click等)

 我们可以称native为原型绑定。只有**使用vue组件时**我们会用到这个修饰符。当我们在组件上绑定监听时，我们绑定的是组件定义的监听。以element框架为例，<el-input>是element提供的组件。

在VUE中，**自定义组件上绑定的事件都是自定义事件(即使绑定@click也会被认为是自定义事件)**，无法直接绑定原生事件(如click)，调用自定义事件需要在组件内使用$emit()的方式

如果element没给el-input内部的原生input标签定义@click =$emit('click'),function)，那么我们这次绑定是无效的。

![img](VUE.assets/15214039-42ceaeceb351369a.webp)

当我们加上native词缀，<el-input @click.native="">

加了native相当于**把自定义组件看成了html元素**可以**直接监听原生click事件(相当于给自定义组件的根元素添加了原生监听事件)**

(**部分组件不需要.native是因为组件已经替你封装好了方法(猜测是在子组件，原生标签上定义$emit('click')，则会调用外部使用组件时在组件上定义的@click事件))**

加了native相当于**把自定义组件看成了html元素**可以**直接监听原生click事件**,否则**自定义组件上绑定的事件默认是自定义事件**(自定义事件通过组件内部使用$emit进行调用)而你在自定义组件上没有定义这个事件,所以**不加native不会执行**





#### .self字段

阻止自己身上冒泡行为的触发

" .stop “和” .self “的区别：” .self “只会阻止自己身上冒泡行为的触发，并不会真正阻止 冒泡的行为。” .stop "会阻止所有的冒泡的行为。(.self修饰的元素的父元素仍会触发冒泡事件)



#### .prevent

阻止默认行为





#### 接收事件对象($event)

![63776600461](VUE.assets/1637766004615.png)

####  v-on事件修饰符

![63776653583](VUE.assets/1637766535835.png)

#### 按键修饰符

![63776723723](VUE.assets/1637767237236.png)



#### v-model   双向绑定

![63777151072](VUE.assets/1637771510728.png)

![63777154432](VUE.assets/1637771544320.png)

![63777193654](VUE.assets/1637771936543.png)

![63777190060](VUE.assets/1637771900607.png)



![63777220564](VUE.assets/1637772205647.png)



#### v-model修饰符

![63785109313](VUE.assets/1637851093136.png)

v-text  v-html

![63785157117](VUE.assets/1637851571178.png)



#### v-show  v-if

![63785205807](VUE.assets/1637852058071.png)

![63785214816](VUE.assets/1637852148164.png)



#### V-for 

![63785384336](VUE.assets/1637853843360.png)



#### 补充：js里引入图片

![63785492767](VUE.assets/1637854927672.png)



#### v-for 更新检测

**单纯改变数组中某个元素的值，不会触发更新，想要触发更新可采用this.$set()修改值**

![63800031797](VUE.assets/1638000439650.png)





#### 虚拟dom

![63800353300](VUE.assets/1638003533004.png)

![63800351554](VUE.assets/1638003515542.png)

![63800369694](VUE.assets/1638003696946.png)

#### v-for  :key作用

![63801001016](VUE.assets/1638010010167.png)

值为索引时，li中的输入框还在原位置，没有跟随"老二，老三"，因为**diff算法**更新时尽量复用原标签，数组插入值，原索引对应的值发生了变化，更新时尽量复用原标签，所以只更新了对应的value，后面的输入框没有更新还在原位置



值为自定义id时，插入新数据，原数据对应的id不变，更新时尽量复用原标签，所以"老二，老三"对应的li标签不改变，直接插入新li，所以原值为“2，3”的输入框能够继续跟随"老二，老三"

![63801014861](VUE.assets/1638010148617.png)

![63801005266](VUE.assets/1638010052666.png)

![63801007642](VUE.assets/1638010076420.png)



#### 动态class

![63801785061](VUE.assets/1638017873686.png)

![63801789207](VUE.assets/1638017892079.png)

#### 动态style

![63801816306](VUE.assets/1638018163065.png)

 

#### 过滤器

![63808586786](VUE.assets/1638085867862.png)

#### 全局方式定义过滤器  Vue.filter

打包入口main.js中定义

![63808597015](VUE.assets/1638085970151.png)

#### 局部方式定义过滤器  filters{}

![63808603157](VUE.assets/1638086031574.png)

#### 过滤器传参 or 多过滤器

![63808627789](VUE.assets/1638086277890.png)

![63808630806](VUE.assets/1638086308060.png)



#### 计算属性 computed

![63808788985](VUE.assets/1638087889851.png)

![63808792816](VUE.assets/1638087928162.png)

#### 计算属性_缓存

计算属性的优势：有缓存，依赖项不变，从缓存中取值，减少运算

![63808816747](VUE.assets/1638088167471.png)

arry.reduce(),可用于计算累加累减

![63808989102](VUE.assets/1638089891020.png)

toFixed(n) 保留n位小数



#### 计算属性_完整写法

用**v-model双向绑定**给计算属性赋值或取值时，要用完整写法，给计算属性添加get set方法

![63809092222](VUE.assets/1638090922221.png)

![63809090762](VUE.assets/1638090907620.png)

补：arr.every（）遍历每个值，当有不符合条件的值时直接返回



#### 侦听器_watch

![63809308696](VUE.assets/1638093086969.png)

#### 深度侦听(侦听复杂数据类型 对象等)

![63809509634](VUE.assets/1638095096343.png)



#### 组件_基本使用

![63810670407](VUE.assets/1638106704078.png)

![63810774419](VUE.assets/1638107744195.png)

组件命名和引用

![64224322698](VUE.assets/1642243226985.png)



#### style 的scoped的原理

![63810689374](VUE.assets/1638106893745.png)

### 组件通信

#### 父组件 →子组件 传值

![63811078093](VUE.assets/1638110780934.png)

**先在子组件props中定义接收的变量**

![64224456453](VUE.assets/1642244564531.png)

![63811070023](VUE.assets/1638110700238.png)

![63811057555](VUE.assets/1638110575558.png)

![64224461530](VUE.assets/1642244615302.png)

![64224465900](VUE.assets/1642244659001.png)





#### 单向数据流 

![63811199445](VUE.assets/1638111994453.png)



#### 组件通信 子向父

![63811351241](VUE.assets/1638113512415.png)



**在父组件内，对引入的子组件绑定自定义事件和事件处理函数**

![63811357703](VUE.assets/1638113577034.png)





**在子组件内,用$emit() 触发调用父组件的自定义事件**

![63811366912](VUE.assets/1638113669120.png)



![64613430851](VUE.assets/1646134308513.png)

![64613437290](VUE.assets/1646134372900.png)

##### 在用emit完成子向父组件传参，只有**单个参数时，在父组件可以用 $event指代子组件向父组件传递的那个参数(在自定义事件里，event代表传参)**

补：关于原生JavaScript传递event事件的内容，

1. 在最早的HTML事件处理程序中我们可以直接传入多个参数，并且可以不传入event直接传参，event可以调换前后顺序但必须是event关键字，不能是e ，如果是e就会报错：ReferenceError: e is not defined

2. 在"οnclick=function"以及addEventListener中不再看重event关键词，可以传任意标识，但只能传event(因为addEventListener三个参数，第一个事件名，第二个传递回调函数，第三个配置，无法直接传递其他参数)，默认只传入`event`参数，如果想传入其它参数，可以采用将方法进行封装来处理

  原文链接：https://blog.csdn.net/a1059526327/article/details/106392305/

**在例如上述，当传递给子组件的参数既要使用又要修改时，要先用props向子组件传递参数，然后在子组件里用$emit()调用事件修改数据，比较麻烦**，可以修改为v-model写法 如下：

![64614606541](VUE.assets/1646146065419.png)

v-model,相当于 :value="xxx"(向子组件传递了名为value的参数),且@input="xxx=$event"(声明了名为input的事件用来修改数据)，子组件需要在props声明value用于接收数据，修改数据则调用input事件

但value作为传递的参数名，语义不清晰，可以在**子组件**自定义v-model参数

prop定义子组件接收的参数名(自定义后就可以不用声明为value),还有上述的@input事件名，可以更改为自定义事件名

![64614835285](VUE.assets/1646148352858.png)

**.sync** 多个数据需要实现上述v-model的效果(一个组件里只能用一个v-model)，用.sync

![64614935142](VUE.assets/1646149351420.png)

.sync用法

// 子组件 update:固定写法 (update:props名称, 值)
this.$emit('update:showDialog', false) //触发事件
// 父组件 sync修饰符
<child  :showDialog.sync="showDialog" />

#### 跨组件传值  EventBus

在EventBus/ 中创建空白vue对象，两个组件共同引入该空白对象，

接收值的组件定义$on()监听事件

传值的组件调用$emit()触发事件

![63811512617](VUE.assets/1638115126178.png)

![63811551625](VUE.assets/1638115516250.png)

![63811553495](VUE.assets/1638115534950.png)





## 构子函数

![63836517438](VUE.assets/1638453059335.png) 

#### 初始化

![63836683362](VUE.assets/1638453038291.png)



![63836691332](VUE.assets/1638453129413.png)

![63836696989](VUE.assets/1638453155655.png)

#### 挂载

![63836860442](VUE.assets/1638453202013.png)

![63836918378](VUE.assets/1638453264800.png)

![63836921510](VUE.assets/1638453290120.png)

![63836923358](VUE.assets/1638453309956.png)

#### 更新

![63836949558](VUE.assets/1638453391286.png)

![63845344858](VUE.assets/1638453448589.png)

#### 销毁

![63836969365](VUE.assets/1638453481244.png)

![63837009304](VUE.assets/1638453530424.png)



#### axios

![63837196503](VUE.assets/1638453563189.png)

#### 可以用扩展运算符...

![63845367419](VUE.assets/1638453674190.png)



#### 配置全局默认基地址

![63845410253](VUE.assets/1638454102533.png)



#### ref获取原生dom元素

![63845430078](VUE.assets/1638454300786.png)

#### ref作用在组件上时，可以获取组件对象实例(用实例可以调用组件方法)

![63845524398](VUE.assets/1638455243987.png)



#### vue更新DOM异步问题

![63845560851](VUE.assets/1638455608512.png)

#### 使用$nextTick解决dom异步更新问题

![63845581993](VUE.assets/1638455819936.png)

![63845569665](VUE.assets/1638455696651.png)



$nextTick

![63845673489](VUE.assets/1638456734897.png)



#### 组件里name属性作用

![63845710837](VUE.assets/1638457108373.png)



新知识点，对props接收的变量进行校验

![63845781657](VUE.assets/1638457816579.png)

![63845791777](VUE.assets/1638457917777.png)

#### 将axios挂载到全局vue

![63845828118](VUE.assets/1638458281189.png)





#### 动态组件

<component :is=""></component> 内置组件

变量名comName可以随意 属性值必须是想要展示的并且已注册的**组件名称**

![63854176603](VUE.assets/1638541766030.png)



#### 组件缓存

![63854258180](VUE.assets/1638542581803.png)

![63854260378](VUE.assets/1638542603785.png)



#### 组件激活和非激活(新生命周期方法)

![63854282019](VUE.assets/1638542820191.png)



#### 组件插槽

<slot></slot>

![63854373075](VUE.assets/1638543730756.png)

![63854374690](VUE.assets/1638543746906.png)



#### 一个组件多个slot

![63854568739](VUE.assets/1638545687391.png)

![63854579575](VUE.assets/1638545795759.png)



#### 作用域插槽

![63862345736](VUE.assets/1638623457367.png)

**上述"scope"可以随意命名，该变量名会自动绑定slot上所有属性和值**

![63862361606](VUE.assets/1638623616062.png)



#### 自定义指令

![63862663019](VUE.assets/1638626630197.png)

**全局**

![63862687425](VUE.assets/1638626874253.png)

**局部**

![63862692203](VUE.assets/1638626922033.png)

![63862705349](VUE.assets/1638627053492.png)



#### inserted指令所在标签被插入网页时执行，update指令对应数据或标签更新时执行

![63862766275](VUE.assets/1638627662750.png)

![63862771228](VUE.assets/1638627712287.png)



父传子，props接收值进行自定义校验 validator

![63862908787](VUE.assets/1638629087879.png)





## 路由

#### vue-router

基本使用

引入   路径@表示src的绝对地址

![63871535439](VUE.assets/1638715354399.png)

全局注册，设置规则

![63871517483](VUE.assets/1638715174831.png)

component里也可以写function

![63992398423](VUE.assets/1639923984238.png)

生成路由对象

![63871523160](VUE.assets/1638715231607.png)

路由对象注入vue实例

![63871528332](VUE.assets/1638715283326.png)

设置挂载点

![63871548733](VUE.assets/1638715487336.png)

![63871569670](VUE.assets/1638715696708.png)

hash值 指url上#后面内容

![63871575485](VUE.assets/1638715754850.png)



### 声明式导航

#### router-link (自动赋予被点击的active类名(可用来设置点击高亮))

![63871591869](VUE.assets/1638715918693.png)

![63871620039](VUE.assets/1638716200398.png)

#### 声明式导航-跳转传参

![63871641170](VUE.assets/1638716411708.png)

![63871656112](VUE.assets/1638716561120.png)

方式2：在路由规则上定义

![63871692124](VUE.assets/1638716921240.png)

![63871686513](VUE.assets/1638716865131.png)

![63871698878](VUE.assets/1638716988784.png)

参数后加?，代表参数非必传	

#### 路由重定向

默认打开网页时，重定向到某个页面

添加匹配规则即可   redirect：

![63871726358](VUE.assets/1638717263582.png)



#### 路由404设置

![63871810879](VUE.assets/1638718108799.png)

#### 路由模式

![63871829079](VUE.assets/1638718290798.png)



#### 编程式导航

![64062241345](VUE.assets/1640622413454.png)

![63871868705](VUE.assets/1638718687058.png)

传参

![63871887808](VUE.assets/1638718878086.png)

![63871865781](VUE.assets/1638718657815.png)

方式2：添加name属性，用name属性进行跳转(需要修改路径时只需修改路由规则)

![63871881261](VUE.assets/1638718812612.png)

#### 编程式导航-跳转传参

使用path方式跳转，只能使用query方式传参

使用name方式跳转，则两者都可选

![63871919369](VUE.assets/1638719193691.png)



#### 路由开启props传参

开启后可把路由参数映射到对应子组件props

![64597483597](VUE.assets/1645974835974.png)

#### 路由嵌套

![63872132866](VUE.assets/1638721328667.png)



#### 声明式导航，active激活类名区别

精确匹配和模糊匹配

![63887522398](VUE.assets/1638875223986.png)

如：

![63887527702](VUE.assets/1638875277029.png)





#### 路由守卫(权限判断)

![63887639899](VUE.assets/1638876398990.png)

![63887631107](VUE.assets/1638876311077.png)



#### 路由懒加载

![65201365183](VUE.assets/1652013651835.png)

通过函数返回的这种写法，在打包时，会把每个模块打包成单独的chunk包，引入时更有效

## vant组件库

![63887653864](VUE.assets/1638876538644.png)

引入所有

![63887698646](VUE.assets/1638876986466.png)

手动按需引入   引入组件 引入样式 手动注册

![63887846608](VUE.assets/1638878466089.png)



自动按需引入

babel-plugin-import

![63887884737](VUE.assets/1638878847370.png)

 

#### meta 元信息 保存路由对象额外信息

![63906081158](VUE.assets/1639060811580.png)

![63906087493](VUE.assets/1639060874937.png)

![63906083776](VUE.assets/1639060837762.png)



#### 二次封装axios和请求api(统一管理 避免请求散落在各个文件)

![63906337548](VUE.assets/1639063375483.png)

封装utils

![63906342957](VUE.assets/1639063429571.png)

封装每个页面对应的api请求

![63906350407](VUE.assets/1639063548132.png)

统一到index.js向外导出

![63906363581](VUE.assets/1639063696739.png)

![63906380940](VUE.assets/1639063809406-1639063810717.png)



Vant组件适配(px自动转成rem)

![63941108042](VUE.assets/1639411080425.png)

![63975384648](VUE.assets/1639753846482.png)

![63975387927](VUE.assets/1639753879276.png)

##### rootValue：设置根元素大小，一般设计为设计稿宽度的十分之一(375宽度设置37.5，750设置75)

项目中引用了amfe-flexible,动态地将1rem设置为屏幕宽度的10分之一(设置不同视窗下根节点的font-size)，而postcss则用来将px转化为rem数值(将代码里的px转为rem单位)，因为引用了amfe-flexible(十分之一)，所以postcss的rootValue设置为设计稿宽度的10分之一

vant按照37.5设计 (头条项目中按照750二倍图设计，因此除vant组件外，rootvalue为75)



![63991949494](VUE.assets/1639919494946.png)

{file} 解构赋值写法

![64000936080](VUE.assets/1640009360805.png)



#### elementui  -select下拉框 v-model绑定获取选中的选项值

在select属性上加v-model绑定变量 可以获取下拉框的选中值

![63975902587](VUE.assets/1639759025870.png)



# Vue 中input或select绑定数字类型时的坑

要绑定数字类型，对应的value属性要加上v-bind, :value，不然还是字符类型

![image-20211218094858502](assets/image-20211218094858502.png)

flex-shrink:0 不参与flex布局宽度计算(某些情况下，对flex某个子元素单独设置宽度不起作用，宽度已被其余子元素分配完，可设置shrink：0让该子元素不参与flex的宽度计算)



#### $toast() 防止多次点击

 this.$toast.loading({

​        duration: 0, // 持续展示 toast

​        message: '操作中...',

​        forbidClick: true // 是否禁止背景点击

​      })

#### vant list组件几个属性含义

![64623246870](VUE.assets/1646233280522.png)

#### vant中List组件immediate-check

![64623182418](VUE.assets/1646231824182.png)

特殊情况，如使用了propub弹出层，弹出层里放置了list组件，若在lisi组件的created()中调用了加载数据的方法，在点击弹出弹出层时，由于判断了offset(其实还有隐形条件即loading为false)，组件自动加载了load方法，created里又调用了一次load方法，会调用两次

解决方法：immediate-check：false，关闭初始化时的滚动位置检查

另一种特殊情况：

![64623233708](VUE.assets/1646232337082.png)

![64623236522](VUE.assets/1646232365222.png)

#### Propub弹出层内容是懒渲染的

只有第一次弹出的时候会加载并渲染内容，弹出层关闭后内容还在，第二次点开不会重新加载

（比如里面用到了list组件，虽然传递过去的变量发生了改变，但list组件并不会自动重新加载数据，除非设置监听，在监听里进行修改，但比较麻烦，之间用v-if让整个组件在弹出层关闭时销毁，打开时重新加载）

若需要每次重新加载，将内容部分加上v-if(v-if为false会销毁内容，下一次为true重新加载)

![64623414830](VUE.assets/1646234148305.png)



#### 依赖注入 Provide(给所有后代组件传值)

![64627464491](VUE.assets/1646274644913.png)

#### 裁切工具库cropperjs



### Webpack proxy反向代理解决跨域问题

> 采用vue-cli的代理配置

vue-cli的配置文件即**`vue.config.js`**,这里有我们需要的 [代理选项](https://cli.vuejs.org/zh/config/#devserver-proxy)

```js
module.exports = {
  devServer: {
   // 代理配置
    proxy: {
        // 这里的api 表示如果我们的请求地址有/api的时候,就出触发代理机制
        // localhost:8888/api/abc  => 代理给另一个服务器
        // 本地的前端  =》 本地的后端  =》 代理我们向另一个服务器发请求 （行得通）
        // 本地的前端  =》 另外一个服务器发请求 （跨域 行不通）
        '/api': {
        target: 'www.baidu.com', // 我们要代理的地址
        changeOrigin: true, // 是否跨域 需要设置此值为true 才可以让本地服务代理我们发出请求
         // 路径重写
        pathRewrite: {
            // 重新路由  localhost:8888/api/login  => www.baidu.com/api/login
            '^/api': '' // 假设我们想把 localhost:8888/api/login 变成www.baidu.com/login 就需要这么做 
        }
      },
    }
  }
}
```

以上就是我们在vue-cli项目中配置的代理设置

**生产环境的跨域**

生产环境表示我们已经开发完成项目，将项目部署到了服务器上,这时已经没有了vue-cli脚手架的**`辅助`**了，我们只是把打包好的**`html+js+css`**交付运维人员，放到**`Nginx`**服务器而已,所以此时需要借助**`Nginx`**的反向代理来进行

```bash
server{
    # 监听9099端口
    listen 9099;
    # 本地的域名是localhost
    server_name localhost;
    #凡是localhost:9099/api这个样子的，都转发到真正的服务端地址http://baidu.com
    location ^~ /api {
        proxy_pass http://baidu.com;
    }    
}
```

**`注意`**:这里的操作一般由运维人员完成,需要前端进行操作,这里我们进行一下简单了解





#### 注册自定义指令

指令功能，给img标签绑定onerror函数(img标签加载地址错误时，会自动执行onerror函数，这里绑定onerror函数设定要执行的操作)，当图片加载失败时，使用默认图片

![64707680294](VUE.assets/1647076802943.png)



#### functional为true，表示该组件为一个函数式组件

函数式组件： 没有data状态，没有响应式数据，只会接收props属性， 没有this， 他就是一个函数



#### 利用sync修饰符关闭新增弹层

> vuejs为我们提供了**`sync修饰符`**，它提供了一种简写模式 也就是

```js
// 子组件 update:固定写法 (update:props名称, 值)
this.$emit('update:showDialog', false) //触发事件
// 父组件 sync修饰符
<child  :showDialog.sync="showDialog" />

```

只要用sync修饰，就可以省略父组件的监听和方法，直接将值赋值给showDialog



#### URl value字符串包含了=或&，需要进行转义

Url参数[字符串](https://so.csdn.net/so/search?q=%E5%AD%97%E7%AC%A6%E4%B8%B2&spm=1001.2101.3001.7020)中使用key=value键值对这样的形式来传参，键值对之间以&符号分隔，如/s?q=abc& ie=utf-8。如果你的**value字符串中包含了=或者&，那么势必会造成接收Url的服务器解析错误**，因此必须将引起歧义的&和= 符号进行转义，也就是对其进行编码。



#### :key 属性的另一个作用

:key值发生变化时，组件会重新渲染，某些情况下，由于组件没有重新渲染导致的bug，可以利用:key



#### // 开启域名/ip访问

​    config.devServer.disableHostCheck(true)



## 注意：用$emit修改父组件传过来的Props值，修改后在当前函数结束前不会立即生效，当前函数内拿到的Props值仍然是修改前的

若该函数内后续有需要用到修改后的值，则建议修改前创建一个中间变量newVal记录修改后的值

```js
Props:{
    checked  //传值为false
},
const changeChecked = () => {
      //创建中间变量newVal记录修改后的值
      const newVal = !checked.value
      emit('updateChecked', newVal) //newVal:true
      // 此时在当前函数结束前，checked值仍为原值 false,但父组件数据已完成修改
       console.log(checked.value) //false
       //若该函数内后续有需要用到修改后的值，则建议修改前创建一个中间变量newVal
      
    }
```