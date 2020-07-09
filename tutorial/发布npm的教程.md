# 发布npm的教程

注：初始化vue-toast-c

```javascript
vue init webpack-simple vue-toast-c
cd vue-toast-c
```

**一：npm命令登录**

```javascript
npm login  // 帐号：htzyl206/3*****8z****t
```

**二：npm发布到官方网上**

```javascript
npm publish
```

> PS :每一次发布新的一版，version版本要改变，不然npm会给我报错。一般情况下，一旦你要修改你已经发布后的代码，然后又要执行发布操作，务必到package.json里面，把version改一下，比如从1.0.0改为1.0.1，然后在执行`npm publish`，这样就可以成功发布了。

**三： 支持插件多种引入方式**

`output`中增加如下配置

```bash
library: 'cacheGet', // 暴露的变量
libraryTarget: 'umd' // 支持的方案，一般填这个就行
```

**四： 处理依赖包**

比如你的插件依赖axios不想将其打包（体积过大）或者用户配置文件等

*webpack.config.js* 增加如下配置

```
externals: {
  'axios':'axios'
}
```

> 注意：将`node_module`文件夹放到忽略文件里