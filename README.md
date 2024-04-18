# 替换.gitmodules中的url

我们在国内使用github上的项目经常会遇到git clone https_url出现无法下载的问题，解决的办法可以使用代理或者镜像库，而我通常使用git clone ssh_url的方式。

这些项目有很多会依赖于其他的项目，并通过git submodule的方式管理。

在我们拉取下来一个项目时，这些依赖关系已经写入到.gitmodules文件中了，而且url通常都是http url，这样我们依然无法通过git submodule相关命令拉取子项目。

我们通过修改.gitmodules的http url为ssh url，这样就可以成功拉取下子项目。

## 如何使用

把replace_url脚本拷贝到你的项目的.gitmodules同一路径下，然后使用replace_url脚本中的do_replace_url函数完成url的替换，你可能要多次执行do_replace_url（因为如果是嵌套的包含子项目，每个子项目下载下来之后都要进行替换，那样才能进一步下载到下一层的子项目），具体来说是这样的：

```bash
# source 脚本
source replace_url
# 之后我们可以使用do_replace_url函数了
do_replace_url
# 这里完成本轮的url替换
# 接着我们进一步下载子项目
git submodule update --init --recursive
# 假如下载了新的子项目，那么继续完成替换
# ....
do_replace_url
git submodule update --init --recursive
# 直到所有的子项目下载完了
# 也就是 git submodule update --init --recursive 命令没有任何输出
# 如果有输出 “正克隆到 'xxx'"就说明没有下载完

```

## 测试

testcase脚本用于测试replace_url中的函数，里面包含一些测试用例。

## 问题

如果脚本使用过程中有任何问题，欢迎提issue。


















