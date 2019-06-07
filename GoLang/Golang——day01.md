# Golang——day01

1. 编译go代码：go build *.go
2. 执行go代码：go run *.go

#### 数据类型

数据类型的作用：告诉编译器这个变量应该以多大的内存存储

##### 命名

Go语言中的函数名、变量名、常量名、类型名、语句标号和包名等所有的命名，都遵循一个简单的命名规则：$\color{red}{一个名字必须以一个字母（Unicode字母）或下划线开头}$，后面可以跟任意数量的字母、数字或下划线。大写字母和小写字母是不同的。

##### Go语言的关键字

| 关键字   | 关键字      | 关键字 | 关键字    | 关键字 |
| -------- | ----------- | ------ | --------- | ------ |
| break    | default     | func   | interface | select |
| case     | defer       | go     | map       | struct |
| chan     | else        | goto   | package   | switch |
| const    | fallthrouth | id     | range     | type   |
| continue | for         | import | return    | var    |

内建常量：true、false、iota、nil

内建类型：int、int8、int16、int32、int64

​		   uint、uint8、uint16、uint32、uint64

​		   uintptr、float32、float64、complex128、complex64

​		   bool、byte、rune、string、error

内建函数：make、len、cap、new、append、copy、close、delete、complex、real、imag、panic、recover

#### 变量

程序运行期间可以改变的量

变量的申明方式：var name type

##### 注意：

1. 导入包，必须要使用
2. 变量声明了，必须要使用
3. 只有声明没有初始化的变量，int类型默认值为0

#### 常量

程序运行期间可以不能改变的量，常量的声明需要const

##### 注意：

1. 常量自动推到类型直接使用“=”，不能使用“:=”

### 函数类型

在Go语言中，函数也是一种数据类型，我们可以通过type来定义它。





## 工具快捷键

IDEA中上下移动一行代码：Ctrl + Shift + 上下键

窗口最大化：Ctrl+Shift+F12 