# Vue 规范

## 一、UI框架及css预处理器选择

1、PC端Vue项目UI框架：ElementUI（优先）、iView
 2、移动端Vue项目UI框架：mint-ui（优先）、vant
 3、sass/scss、less、stylus     ？
 推荐less：less相对比sass更简洁，而stylus的语法灵活性开放性过大，不利于团队协作开发。

## 二、命名规范

### 2.1、通用命名方法

| 命名方法                 | 举例          |
| ------------------------ | ------------- |
| Camel命名法              | thisIsAnApple |
| Pascal命名法             | ThisIsAnApple |
| kebab-case短横线分隔命名 | bus-box       |

### 2.2、样式命名

- id和class的命名原则
   应反映该元素的功能或使用通用名称，而不要用抽象的晦涩的命名（原则：见名知其义）

```csharp
//bad
.hljs-comment .js-evernote-checked {
  background:red;
}
//bad
.comment .evernote-checked {
  background:red;
}
```

### 2.3、文件命名

| 名称               | 命名方法     | 说明            | 举例                |
| ------------------ | ------------ | --------------- | ------------------- |
| 文件夹             | kebab-case   | 过长用减号-连接 | auto-complete       |
| 文件-样式          | kebab-case   | 过长用减号-连接 | page-style.less     |
| 文件-index（特例） | 全小写       | 方便省略引入    | page/index.jsx      |
| 文件-组件          | Pascal命名法 |                 | CalenderCustome.vue |
| 扩展名             | .vue         | 统一命名        | page.vue            |

### 2.4、Vue 项目中的命名



```undefined
* Store 中的Module 使用“小驼峰命名法”
* Store 中的Mutation 使用 全部大写的下划线命名法
* Store 中的state/getters/action 使用“小驼峰命名法”
* 前端路由路径使用全小写命名法
```

### 2.5 方法命名

1. 动宾短语（good：jumpPage、openCarInfoDialog）（bad：go、nextPage、show、open、login）
2. ajax 方法以 get、post 开头，以 data 结尾（good：getListData、postFormData）（bad：takeData、confirmData、getList、postForm）
3. 事件方法以 on 开头（onTypeChange、onUsernameInput）
4. 尽量使用常用单词开头（set、get、open、close、jump）
5. 驼峰命名（good: getListData）（bad: get_list_data、getlistData）

### 2.6 紧密耦合的组件名

**和父组件紧密耦合的子组件应该以父组件名作为前缀命名。**
 如果一个组件只在某个父组件的场景下有意义，这层关系应该体现在其名字上。因为编辑器通常会按字母顺序组织文件，所以这样做可以把相关联的文件排在一起。

```cpp
//bad
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue
//good
components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue

//bad
components/
|- SearchSidebar.vue
|- NavigationForSearchSidebar.vue
//good
components/
|- SearchSidebar.vue
|- SearchSidebarNavigation.vue
```

## 三、排版规范

### 3.1、多个特性的元素，每个特性应该单独一行

```xml
<el-select  v-model="value" placeholder="请选择">
    <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value">
    </el-option>
</el-select>
```

### 3.2 方法顺序

- components
- props
- data
- created
- mounted
- activited
- update
- beforeRouteUpdate
- metods
- filter
- computed
- watch

## 四、项目目录结构

### 4.1、项目初始目录

```csharp
├── node_modules  // 项目依赖包文件夹
├── build   // 编译配置文件，一般不用管
│   ├── build.js
│   ├── check-versions.js
│   ├── dev-client.js
│   ├── dev-server.js
│   ├── utils.js
│   ├── vue-loader.conf.js
│   ├── webpack.base.conf.js
│   ├── webpack.dev.conf.js
│   └── webpack.prod.conf.js
├── config  // 项目基本设置文件夹
│   ├── dev.env.js  // 开发配置文件
│   ├── index.js  // 配置主文件
│   └── prod.env.js  // 编译配置文件
├── index.html   // 项目入口文件
├── package-lock.json  // npm5 新增文件，优化性能
├── package.json  // 项目依赖包配置文件
├── src    // 我们的项目的源码编写文件
│   ├── App.vue  // APP入口文件
│   ├── assets  // 初始项目资源目录，回头删掉
│   │   └── logo.png
│   ├── components // 组件目录
│   │   └── Hello.vue // 测试组件，回头删除
│   ├── main.js // 主配置文件
│   └── router // 路由配置文件夹
│       └── index.js   // 路由配置文件
└── static // 资源放置目录
```

### 4.2 src目录配置

```cpp
├── App.vue // APP入口文件
├── components  // 组件文件夹
|——assets  //资源文件夹
├── main.js  // 项目配置文件
├── router  // 路由配置文件夹
│   └── index.js  // 路由配置文件
├── views  // 我们的页面组件文件夹（待定，选用less或scss）
│   ├── home1   // 页面1文件夹
│      └── hello.vue 
│   ├── home2   // 页面2文件夹
│   └── Login.vue   // 一个页面无需文件夹
```

## 五、语法

### 5.1 .v-for 循环必须加上 key 属性

v-for 循环必须加上 key 属性，在整个 for 循环中 key 需要唯一

```xml
<el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value">
</el-option>
```

### 5.2 组件的 data 必须是一个函数。

```kotlin
//bad
export default {
  data: {
    foo: 'bar'
  }
}
//good
export default {
  data () {
    return {
      foo: 'bar'
    }
  }
}
```

## 六、约定

### 6.1、页面跳转

对没有任何业务逻辑控制的页面，可以直接跳转。
 对于有业务逻辑控制的页面，应绑定链接事件方式，在事件中需要显示loading层，转到目标页面后，再隐藏loading层。

### 6.2、注释

1.各组件中重要函数或者类说明,复杂的业务逻辑处理说明
 函数注释，可使用块级注释

```dart
/*
    *函数注释
    *@ param {string} p1 参数1的说明
    *@ param {string} p2 参数2的说明，比较长
    *@ return 返回值描述
    */
    aa() => {
        alert();
    }
```

2.注释块必须以/**（至少两个星号）开头**/；
 3.单行注释使用//；
 4.文件头部注释

```cpp
/* ------------------------------------------------------------
    author : cattleya
    create:2016-12-30
    descreption:recharge相关样式（包含input/label/button）

    ------------------------------------------------------------ \*/
```

### 6.3  细节

1.使用ES6风格编码源码
 2.定义变量使用let ,定义常量使用const,使用export ，import 模块化
 3.避免 this.$parent
 4.谨慎使用 this.$refs
 5.无需将 this 赋值给 component 变量
 6.调试信息console.log() debugger 使用完及时删除
 7.常量全部大写，并使用下划线连接  ，示例：const MAX_COUNT = 10;

## 七、编辑器

### 7.1 推荐vscode

### 7.2 设置常用设置

a.首先点击文件/首选项/设置
 b.点击上面的...，选择打开settings.json
 c.选中用户设置
 d.推荐使用如下配置进行覆盖默认用户配置

```json
{
"git.ignoreMissingGitWarning": true,
"window.restoreWindows": "all",
"editor.fontSize": 24,
"files.autoSave": "onFocusChange",
"explorer.confirmDragAndDrop": false,
"explorer.confirmDelete": false,
"vetur.format.defaultFormatter.js": "vscode-typescript",
"vetur.format.defaultFormatter.html": "js-beautify-html",
"vetur.format.defaultFormatter.ts": "vscode-typescript",
"vetur.format.defaultFormatter.css": "prettier",
"prettier.disableLanguages": [],
"prettier.tabWidth": 4,
"editor.detectIndentation": false,
"beautify.tabSize": 4,
"workbench.colorTheme": "Visual Studio Dark",
}
```

### 7.3 推荐常用插件

1.Beautify   格式化代码
 2.HTML CSS Support 智能提示CSS类名以及id
 3.HTML Snippets  智能提示HTML标签，以及标签含义
 4.Vetur  Vue多功能集成插件，包括：语法高亮，智能提示，emmet，错误提示，格式化，自动补全，debugger。vscode官方钦定Vue插件，Vue开发者必备

## 示例 - 单文件

```kotlin
// CarList.vue
<template>
    <div class="container">
        <ul>
            <li v-for="car in carList" :key="car.id">
                <img src="car.logo" alt="">
                <p>{{car.name | empty}}</p>
            </li>
        </ul>
        <button @click="loadNextPage">下一页</button>
        <div class="last" v-show="isLast">已经没有更多了...</div>
        <div class="loading" v-show="isLoading">正在加载...</div>
        <div class="error" v-show="isError" @click="getCarListData">加载错误，点击 <span class="font-blue">这里</span> 重试</div>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                carList: [],
                totalPage: 1, // 总页数
                page: 0, // 当前页数
                isLoading: false, // 是否正在加载
                isError: false // 是否加载错误
            }
        },
        mounted() {
            this.loadNextPage();
        },
        methods: {
            // 获取列表数据
            getCarListData() {
                let data = {
                    page: this.page, // 当前页数
                    pageSize: 10 // 每页条数 
                }

                this.isLoading = true;
                this.isError = false;
                this.$ajaxGet('/car/list', data).then(data => {
                    // 加载成功
                    this.carList.concat(data.list);
                    this.page = data.page;
                    this.totalPage = data.totalPage
                    
                    this.isLoading = false;
                }).catch(() => {
                     //  加载列表失败
                    this.isLoading = false;
                    this.isError = true;
                });
            },
            // 下一页
            loadNextPage() {
                if(this.page <= this.totalPage) {
                    this.page ++;
                    this.getCarListData();
                }
            }
        },
        filters: {
            empty(value) {
                return value || '未知';
            }
        },
        computed: {
            // 是否是最后一条
            isLast() {
                return !this.isLoading && this.carList.length > 10 && !this.isError && this.page >= this.totalPage;
            }
        }
    }
</script>
```

