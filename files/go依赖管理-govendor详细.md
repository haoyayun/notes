## go依赖管理-govendor详细

### 安装
> go get -u github.com/kardianos/govendor

### 基础理念
> go vendor 是go 1.5版本引入的管理包依赖方式，1.6正式引入
>
> 基本思想，将引用的外部包的源代码放在当前工程的vendor目录下，go 1.6版本以后，编译go代码会优先从vendor目录先寻找依赖包

### 解决的问题
> 避免项目代码外部依赖过多，迁移后，需要多次go get 外包依赖包
>
> 通过go get 重新拉去的外部依赖包的版本可能和工程开发时使用的不一致，导致编译错误问题

### 未解决的问题
> 无法精确的引用外部包进行版本控制，不能指定引用某个特定版本的外部包。只是在开发时，将其拷贝过来，但是一旦外部包升级,vendor下的代码不会跟着升级
> 
> vendor下面并没有元文件记录引用包的版本信息，这个引用外部包升级产生很大的问题，无法评估升级带来的风险


## 使用govendor,优化go vendor问题
> govendor是golang的vendor包管理器，方便管理vendor和vendor包

### govendor包类型详细
> 对于govendor来说，主要存在三种位置的包 
> >(local)本地包：项目自身的包组织
> >
> >(external)外部依赖包：传统的放在$GOPATH下的依赖包
> >
> >()vendor包：被govendor管理的放在vendor目录下的依赖包

### 常用命令
|命令|功能|
|:---:|:---:|
|init|初始化vendor目录和vendor.json文件|
|list|列出并过滤现有的依赖项和包[govendor list -v fmt 列出包使用者]|
|add|添加$GOPATH包到vendor目录|
|add PKG_PATH|添加指定的依赖包到vendor目录|
|update|从$GOPATH更新依赖包到vendor目录|
|remove|从vendor管理中删除依赖|
|status|列出多有缺失、过期和修改过的包|
|fetch|添加或更新包到本地vendor目录|
|sync|本地存在vendor.json的时候拉取依赖包，匹配所记录的版本|
|get|类似go get目录，拉取依赖包到vendor目录|

### 状态
|状态|缩写状态|含义|
|:---:|:---:|:---:|
|+local|l|本地包，即项目自身的包组织|
|+external|e|外部包，即被govendor管理，但不在vendor目录下|
|+vendor|v|已被govendor管理，即在vendor目录下|
|+std|s|标准库中的包|
|+unused|u|未使用的包，即包在vendor目录下，但项目并没有用到|
|+missing|m|代码引用了依赖包，但该包没有找到|
|+program|p|主程序包，意味着可以编译为执行文件|
|+outside||外部包和缺失的包|
|+all||所有的包|

---

### 参考
> go 依赖管理利器 -- govendor
> 
> github.com/kardianos/govendor