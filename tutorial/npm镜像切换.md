# npm镜像切换

> nrm是一个NPM源管理器，允许你快速的在多个NPM源间切换
>
> npm cnpm taobao nj
>
> npm Mirror 常用npm源为`npm`、`cnpm`、`tabbao`

```javascript
npm install -g nrm //全局安装
```

2.*列出可选源*

```javascript
nrm ls

  npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
* taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
```

带`*`号的是当前使用的源，我当前使用的是taobao源 3.*切换源*

```
nrm use cnpm

   Registry has been set to: http://r.cnpmjs.org/
```

4.*增加源* 你可以增加定制的源，特别适用于添加企业内部的私有源

```
nrm add <registry> <url> [home]
```

5.*删除源*

```
nrm del <registry>
```

6.*测试速度* 通过`nrm test`来测试相应源的响应时间，来选择选择合适的npm源 - 测试某个源的响应时间

```
nrm test npm
  npm ---- 812ms //网络会影响你的响应时间，所以可以多测几次
```

```
nrm test

  npm ---- 2913ms
* cnpm --- 3041ms
  taobao - 669ms
  nj ----- Fetch Error
  npmMirror  2771ms
  edunpm - Fetch Error
```

# 