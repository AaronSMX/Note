#Kotlin_Day07笔记

构建器模式

(block:类名.()->Unit):给函数参数



# gradle

可以用kotlin代码控制的一种智能的自动化构建工具.(实质:构建并使用工具)

在方法上方添加@Test

断言方法



Java常用构建工具对比:

Ant:2000年 编译测试

Maven:2007年 编译测试,依赖管理

Gradle:2012年 编译测试,依赖管理,DSL自定义扩展任务



网址:gradle.org



# 如何使用gradle

gradle.org -> install gradle -> download -> v4.0后开始支持kotlin -> v4.1比较经典

解压到非中文非空格的目录下

为了在任何地方都能使用gradle -> 配置环境变量:新建gradle的bin目录所在地址,在path中增加gradle的bin目录路径



# 在idea中用kotlin实现gradle

创建gradle文件

配置gradle环境(3步):在build.gradle后加.kts,打开文件改引号

配置环境支持java文件:在src文件下创建main文件夹,在main下创建kotlin文件夹,在build文件中{}内输入application,kotlin("jvm")等 刷新gradle

写代码

发布:在build文件中指定主类名application{}



project是一个接口

project实例可以在配置文件中通过project隐式调用

即application的实质是project.application



# Task

包名 -> 项目名 -> 选择使用本地工具

在build.gradle文件中写代码:

task(任务名){}.dependsOn():任务的依赖,()内传入上一个任务,会接着上一个任务执行

gradle会首先找到每一个任务,执行每一个任务中闭包中的逻辑,这称为任务生命周期

在任务中闭包前加doFirst或doLast关键字,先\后执行该任务及其闭包中的语句再执行其它的

改完build文件的后缀名后关闭重新打开



group



# 默认属性

project.properties获取默认属性,它的实质是一个map集合

遍历默认属性:定义一个任务,将逻辑等级设置为doFirst在其中用foreach遍历



# 增量更新

新建两级文件夹在build文件中plugins{application},在下面并列写代码

需求:查看你的工作量 -> 把源文件的名称写入到一个文件中

写一个拷贝工作量的任务,将逻辑等级设置为doFirst,在其中写代码



//指定输入源和输出源

//获取src下的文件树

//在src文件夹下新建info.txt,接收文件info.txt

//遍历文件树,找到文件

//遍历时判断,如果是文件就将其文件名添加到文档中,并换行



**使用系统插件的方式**plugins{}:插件,可以定义很多任务

application插件:支持java项目的构建和发布java项目应用程序

java插件:只有构建java项目的功能,没有发布的作用

kotlin("jvm")插件:支持kotlin构建和发布的作用



三方开发的插件(插件市场的插件)也可以使用

在gradle.org -> Explore中的Plugins途径下载

**使用第三方插件的方式一**plugins{id("插件名") version("当前版本号")}:插件,可以定义很多任务

**使用第三方插件的方式二**apply{plugin("插件名")} PS:可能会不稳定



eclipse导入jar包的步骤:新建文件夹libs将jar包黏贴进去右键构建工作路径



将java文件中的文件黏贴到kotlin会提示是否转换成kotlin



# 依赖管理

在什么地方以什么形式引入代码,告诉kotlin在什么地方或什么模式

**常见仓库:**maven central\Jcenter\Local本地仓库\文件仓库\自定义maven仓库(最常用)

**声明仓库:**repositories{mavenCentral(),Jcenter()...}

依赖的配置:dependencies{compile(kotlin())}



# 测试驱动开发

# 动态语言和静态语言

**动态语言:**编译完运行后才能知道是否正确,且不知道错误在哪,常见的动态语言有:python,groovy,javascript

**静态语言:**编译时就能及时发现并报错,常见的静态语言有:kotlin,java

# Gradle扩展任务

需求:拷贝java文件夹下所有的文件到temp目录下

打开官网->gradledoc->gradle DSL->copy

按其格式拷贝文件



# 多项目构建

idea中每个project相当于eclipse每个工作空间,module相当于每个项目

在gradle中APP为主运行模块,Core为核心模块

ArtifactID:模块名

APP中的build文件负责当前模块的配置

Core中的build文件负责整个工程的配置

在多模块中创建2级目录并配置kotlin环境

编写代码

测试代码:在统一模块下创建test的2级文件夹,断言

在APP模块中导入Core工程依赖

**优化代码**:

//给每个子项目配置



动手能力不限于敲代码,还可以是学一门新的软件熟悉用法和优缺点,学习一门新的语言等,

能实现什么更多的需求?





