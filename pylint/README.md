# Pylint

Pylint是一个Python静态代码分析工具，它可以查找编程错误、帮助执行编码标准、发现代码异味并提供重构建议，也可以编写自己的插件来添加自己的检查或以某种方式扩展pylint。并且它都基于以下原则：深度、精度和速度。
百度代码扫描团队基于Pylint，结合baidu内部大量的研发实践，对工具中高误报的场景做了优化或降级。
如:
not-an-iterable、unsubscriptable-object等规则对装饰器用法的误报；
undefined-variable对开源框架变量动态加载场景的误报等

## 快速开始

#### 准备工作
下载项目

```
git clone ssh://[帐号]@icode.baidu.com:8235/baidu/codescan/pylint
```
#### 如何构建
保留了pylint的原有构建方式: 
pylint可以通过pip很容易的构建

```
pip install pylint
```
或者通过源码的方式进行安装，进入到pylint根目录，运行
```
python setup.py install
```

参考：[pylint参考手册](https://pylint.readthedocs.io/en/latest/)
## 技术参考

1. **pylint启动类**

&ensp;&ensp;在`pylint.lint`中主要有`pylint.lint.Run`和`pylint.lint.PyLinter`两个类。
- pylint.lint.Run对象用来启动pylint,它会对输入的命令行选项做一些基本检查，以及要使用的配置文件和要指定的插件，之后会创建`pylint.lint.PyLinter`实例并使用我们所指定的命令初始化，最后会创建子进程去并行的执行。
- pylint.lint.PyLinter负责协调pylint进程，它解析配置并为checkers和其他插件提供配置，处理checkers发出的消息，处理输出的报告，并启动checkers。
2. **Checkers**

&ensp;&ensp;所有默认的pylint checks都在`pylint.checkers`中，这是pylint的核心所在。大多数的checkes都是基于AST语法树的，所以我们使用了astroid去处理它，`pylint.checkers.utils`提供了大量的方法去处理astroid。

3. **扩展模块中的可选Pylint检查器**

你可以通过在`.pylintrc`的`MASTER`部分添加一个`load-plugins`来激活这些插件。
**例**：
```
load-plugins=pylint.extensions.docparams,pylint.extensions.docstyle
```

pylint提供了下列插件
- **Deprecated Builtins checker** 
- **Broad Try Clause checker** 
- **Else If Used checker** 
- **Compare-To-Zero checker**  
- **Parameter Documentation checker** 
-  **Docstyle checker**
- **ompare-To-Empty-String checker** 
- **Design checker** 
- **Overlap-Except checker**
- **Multiple Types checker**

**参考**：[pylint扩展插件](https://pylint.readthedocs.io/en/latest/technical_reference/extensions.html#pylint-extensions-overlapping-exceptions)
#### 如何运行
1. 通过包或模块的名称运行pylint
```
pylint [options] modules_or_packages
```
2. 通过文件名运行pylint

```
pylint mymodule.py
```

参数：
- -j 开启Pylint子进程数量 
`pylint -j 4 mymodule1.py mymodule2.py mymodule3.py mymodule4.py` 将产生4个并行Pylint子进程，其中每个提供的模块将被并行检查。
-  \- -ignore=<file[,file...]> 指定的目录或者文件跳过扫描，基于名字而不是路径。
- \- -output-format=<format>, 选择输出的格式（text, json, 自定义格式）。
- \- -list-msgs 生成pylint的消息。
- \- -list-msgs-enabled 显示配置文件中启用和禁用的消息列表。
## 测试

pylint/tests目录下包含测试源码目录。运行`pylint test_self.py`，如果正常会得到类似下面的结果：
![pylint test_self.py](https://img-blog.csdnimg.cn/20191121200941875.png)

## 如何贡献

1. 贡献代码类型
  -- 项目优化与bug修复
  -- 新增扫描规则
2. 贡献代码步骤
-- 添加百度hi讨论组，与项目Owner讨论此需求的必要性
-- 确认需求后，需拉分支进行本地开发，并进行线下测试
-- 测试完成后，提交代码到icode上
-- 项目Owner评审通过并合入
3. 质量要求
-- 项目优化与bug修复必须通过单元测试
-- 提交的代码必须通过icode机器人review（编码检查及bugbye检查）
-- 通过项目Owner的人工review

## 维护者


 Owners：bugbye@baidu.com
## 讨论

百度Hi讨论群: 1548779
 
