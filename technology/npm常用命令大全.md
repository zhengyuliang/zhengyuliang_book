### npm常用命令大全

##### 1.查看版本

```bash
npm -v
```

##### 2.安装模块

**全局安装**

```bash
npm install <模块名> -g
```

**本地安装**

```
npm install <模块名>
```

**在项目中安装保存到package.json中**

```bash
npm install <模块名> --save-dev
```

##### 3.查看安装信息

1. 查看所有安装模块

   ```bash
   npm list -g
   ```

2. 查看某个模块

   ```bash
   npm list <模块名>
   ```

##### 4.卸载模块

```bash
npm uninstall <模块>
```

##### 5.更新模块

```bash
npm update <模块>
```

##### 6.搜索模块

```bash
npm search <模块>
```

##### 7.清空本地缓存

```bash
npm cache clear
```

# 附：

### 使用淘宝 NPM 镜像

大家都知道国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。

淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

这样就可以使用 cnpm 命令来安装模块了

```bash
cnpm install <模块名>
```

