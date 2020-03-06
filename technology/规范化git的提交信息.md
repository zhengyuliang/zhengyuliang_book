## 规范化git commit信息

> 在git的使用中，一种最佳实践是使用格式化的commit信息，这样方便自动化工具进行处理，可以快速生成Release Notes，甚至可以直接对接CI工具进行更进一步的规范化发布流程。

#### Angular的git commit实践

Angular定义的commit基本格式如下:

```markdown
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

> 除了第一行的Header部分必填外，其余均可选。注意Header, Body, Footer中间有空白行分割。

![流程图](https://upload-images.jianshu.io/upload_images/4228468-c18955beb03163df.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

Header部分只有一行，包括三个字段：**`type`（必需）、`scope`（可选）和`subject`（必需）**。

**`type`用于说明 commit 的类别，只允许使用下面7个标识:**

- `feat`：新功能（feature）
- `fix`：修补bug
- `docs`：文档（documentation）
- `style`： 格式（不影响代码运行的变动）
- `refactor`：重构（即不是新增功能，也不是修改bug的代码变动）
- `test`：增加测试
- `chore`：构建过程或辅助工具的变动

通常`feat`和`fix`会被放入changelog中，其他(`docs`、`chore`、`style`、`refactor`、`test`)通常不会放入**changelog**中。

**`scope`用于说明 commit 影响的范围，可选值。通常是文件、路径、功能等**

**`subject`是 commit 目的的简短描述，不超过50个字符。如用英语表述:**

- 以动词开头，使用第一人称现在时，比如`change`，而不是`changed`或`changes`
- 第一个字母小写
- 结尾不加句号（`.`）



#### 自动化工具处理git commit

> commitizen工具，通用的自动化git commit框架

##### 1：NodeJS项目

```
 # 将commitizen命令行安装到全局(也可以用npx替代，或安装到项目的Dev依赖)
npm install commitizen -g

# 使用commitizen命令初始化当前工程为commitizen友好的工程
commitizen init cz-conventional-changelog --save-dev
```

执行完毕后可以发现在`package.json`的`devDependencies`配置多了`cz-conventional-changelog`依赖，同时，也增加了以下配置段:

```json
  "config": {
    "commitizen": {
      "path": "./node_modules/cz-conventional-changelog"
    }
  }
```

此时`git cz`将出现类似于下面的交互式终端:

```npm
cz-cli@3.1.1, cz-conventional-changelog@2.1.0


Line 1 will be cropped at 100 characters. All other lines will be wrapped after 100 characters.

? Select the type of change that you're committing: (Use arrow keys)
❯ feat:     A new feature
  fix:      A bug fix
  docs:     Documentation only changes
  style:    Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
  refactor: A code change that neither fixes a bug nor adds a feature
  perf:     A code change that improves performance
  test:     Adding missing tests or correcting existing tests
(Move up and down to reveal more choices)
```

以后，只要是需要`git commit`的地方，通通替换为`git cz`或`git-cz`即可这样交互式的输入符合Angular commit规范的git log了。其余commit的参数也兼容，比如`-a`, `--amend`等等。

**自动检测commit是否符合规范**

> 考虑在`git commit`的`hook`中加入[commitlint](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fconventional-changelog%2Fcommitlint)检测，不符合commit规范的提交在本地就无法提交进去

在项目依赖中添加`husky`, `commitlint`相关依赖:

```
npm i -D husky @commitlint/config-conventional @commitlint/cli
```

然后在`package.json`配置中添加`husky`的git hook配置:

```
{
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -x @commitlint/config-conventional -E HUSKY_GIT_PARAMS"
    }
  }
}
```

> 巧妙合理利用**husky**的**hook**功能可以大大规范化开发

##### 非NodeJS项目

对于非NodeJS项目，可以将`commitizen`, `commitlint`等这些工具安装为全局的命令行工具分开使用。

安装`commitizen`为全局:

```
npm install -g commitizen cz-conventional-changelog

# 注: powershell使用下面命令产生的文件是UTF16格式，需要手工转为UTF8保存
echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
```

然后像`git commit`那样直接使用`git cz`或`git-cz`就可以弹出交互式提示了:

```js
 git add new.md
 git cz
cz-cli@3.1.1, cz-conventional-changelog@2.1.0


Line 1 will be cropped at 100 characters. All other lines will be wrapped after 100 characters.

? Select the type of change that you're committing: (Use arrow keys)
❯ feat:     A new feature
  fix:      A bug fix
  docs:     Documentation only changes
  style:    Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
  refactor: A code change that neither fixes a bug nor adds a feature
  perf:     A code change that improves performance
  test:     Adding missing tests or correcting existing tests
(Move up and down to reveal more choices)
```

**检测commit是否符合规范**

> 同样可以将`commitlint`安装到全局，以命令行方式在本地手工检测运行。

```
npm install -g @commitlint/cli @commitlint/config-conventional
```

每次commit之后可以使用下述命令检测:

```js
commitlint -x '@commitlint/config-conventional' -e
//-e/--edit参数代表读取git commit最后一条记录

commitlint -x '@commitlint/config-conventional' -f HEAD~1
// -f/--from参数可以指明从哪一条commit记录开始检测，还可以配合-t/--to参数检测一个commit区间段。
```

### 版本发布

规范化commit记录的作用就是为了方便我们知道每次发布之后到底改了什么内容。利用[conventional-changelog](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fconventional-changelog%2Fconventional-changelog)这个工具可以很方便的帮我们产生**changelog**。

```
npm install -g conventional-changelog-cli
```

如果之前每次commit都使用规范化的commit提交，那么使用:

```
conventional-changelog -p angular
```

```
conventional-changelog -i CHANGELOG.md -s -p angular
```

> 终端中看到的内容将输出到`CHANGELOG.md`文件。再次使用上述命令可以将新的change log追加到文件中。可以追加`-r 0`参数代表将commit记录从头至尾全部生成changelog。

#### 更自动化的发布方式standard-version

在[conventional-changelog](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fconventional-changelog%2Fconventional-changelog)的官方文档中，官方更鼓励使用更上层的工具[standard-version](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fconventional-changelog%2Fstandard-version)来产生`CHANGELOG.md`。`conventional-changelog`可以帮助我们快速检查要生成的CHANGELOG.md的格式是否符合期望，而`standard-version`可以自动帮助我们做以下几件事情:

- 升级元数据中的版本号(如`package.json`,`composer.json`等等)
- 使用[conventional-changelog](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fconventional-changelog%2Fconventional-changelog)更新 *CHANGELOG.md*
- 提交`package.json` (如果有) 和 `CHANGELOG.md`
- 给新版本打一个tag

首先安装standard-version到全局命令行:

```
npm i -g standard-version
```

执行下`standard-version`，将看到类似于下面这样的输出:

```
 standard-version
✔ created CHANGELOG.md
✔ outputting changes to CHANGELOG.md
✔ committing CHANGELOG.md
✔ tagging release v2.0.0
ℹ Run `git push --follow-tags origin master && npm publish` to publish
```

### standard-version一些基本用法

直接跟空参运行的行为我们已经看到了，一些基本参数介绍下:

- `--dry-run`: 强烈建议正式运行之前先加这个参数，打印一下要做的事情，并不真正执行
- `--first-release`/`-f`: 首次发布，加上这个参数代表不会升级版本号。仅第一次使用需要
- `--commit-all`/`-a`: 等效于`git commit -a`，建议自己决定要提交哪些文件，都`add`没问题之后再执行`standard-version`
- `--release-as`/`-r`: 手工指定下一个版本号(格式``)。这个参数可能会经常使用。`standard-version`默认规则是从`major`/`minor`/`patch`中最后一个非零的字段开始累加，如`v1.0.0 -> v2.0.0`, `v1.1.0 -> v1.2.0`, `v1.1.1 -> v1.1.2`，指定这个参数可以规避这个规则，从自定义的版本号开始重新按上述规则升级
- `--prerelease`/`-p`: 预发布版本，默认产生的tag像这样: `v1.0.1-0`。可以跟上一个可选的tag id。如`-p alpha`，将产生这样的tag: `v1.0.1-alpha.0`